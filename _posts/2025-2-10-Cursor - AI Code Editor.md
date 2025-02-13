---
layout: post
title: Playing with Cursor AI - Notepad++ Digital Artifacts
author: 'ogmini'
tags:
 - DFIR
 - AI
 - Cursor AI
---

David Cowen recently introduced me to [Cursor](https://www.cursor.com) which is an AI Code Editor and he has a few posts on his blog about using it that you can find [here](https://www.hecfblog.com/search/label/ai%20programming?&max-results=10). I wanted to play around with this and my recent work on researching Notepad++ gave me a good excuse as it provided a target that wasn't too complex. 

Just a few minutes of prompts in the Composer resulted in a simple Python script that will output the "File" elements from the `session.xml` file along with their attributes to a CSV file. It will also compare the "File" elements to the Backup folder and note any differences in another csv file. A few of my prompts:

- Please write a python script to parse the session.xml file. It should pull out all the "File" child elements.
- For each "File" element, read the backupFilePath attribute and verify that file exists at the disk location.
- Please output the files found in the sessions.xml to a csv file.
- Also output a CSV file of any files in the folder but not found in the session.xml
- Prompt the user for the location of the session.xml file.

Sidenote: I don't know why I'm so polite to the AI.

Below is the python code that it generated. I don't primarily code in Python and this would have taken me a bit longer to write manually. 

~~~
import xml.etree.ElementTree as ET
from typing import List, Dict
import os
from pathlib import Path
import csv

def parse_session_files(file_path: str) -> List[Dict[str, str]]:
    """
    Parse the session.xml file and extract all File elements.
    Verify existence of backup files.
    
    Args:
        file_path (str): Path to the session.xml file
        
    Returns:
        List[Dict[str, str]]: List of dictionaries containing file information
    """
    try:
        tree = ET.parse(file_path)
        root = tree.getroot()
        
        files = []
        # Find all File elements regardless of their location in the XML
        for file_elem in root.findall('.//File'):
            file_info = {}
            # Get all attributes of the File element
            for key, value in file_elem.attrib.items():
                file_info[key] = value
            # Get all child elements of the File element
            for child in file_elem:
                file_info[child.tag] = child.text
                
            # Verify backup file exists if backupFilePath is present
            if 'backupFilePath' in file_info:
                backup_exists = os.path.exists(file_info['backupFilePath'])
                file_info['backup_exists'] = backup_exists
                
            files.append(file_info)
            
        return files

    except ET.ParseError as e:
        print(f"Error parsing XML file: {e}")
        return []
    except FileNotFoundError:
        print(f"File not found: {file_path}")
        return []
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        return []

def get_folder_files(folder_path: str) -> List[str]:
    """
    Get all files in the specified folder and its subfolders.
    
    Args:
        folder_path (str): Path to the folder to scan
        
    Returns:
        List[str]: List of absolute file paths
    """
    try:
        folder_files = []
        for root, _, files in os.walk(folder_path):
            for file in files:
                folder_files.append(os.path.abspath(os.path.join(root, file)))
        return folder_files
    except Exception as e:
        print(f"Error scanning folder: {e}")
        return []

def export_mismatches_to_csv(files_not_in_session: set, output_file: str = "files_not_in_session.csv"):
    """
    Export the list of files found in folder but not in session.xml to a CSV file.
    
    Args:
        files_not_in_session (set): Set of Path objects for files not in session.xml
        output_file (str): Name of the output CSV file
    """
    try:
        with open(output_file, 'w', newline='', encoding='utf-8') as csvfile:
            writer = csv.writer(csvfile)
            writer.writerow(['FilePath'])  # Header
            for file_path in sorted(files_not_in_session):
                writer.writerow([str(file_path)])
                
        print(f"\nSuccessfully exported mismatched files to {output_file}")
        
    except Exception as e:
        print(f"Error exporting mismatches to CSV: {e}")

def compare_files(session_files: List[Dict[str, str]], folder_files: List[str]):
    """
    Compare session files with folder files and print mismatches.
    """
    # Convert paths to Path objects for consistent comparison
    folder_paths = {Path(f).resolve() for f in folder_files}
    session_paths = {Path(f['backupFilePath']).resolve() for f in session_files if 'backupFilePath' in f}
    
    # Find mismatches
    files_not_in_session = folder_paths - session_paths
    files_not_in_folder = session_paths - folder_paths
    
    # Print results
    if files_not_in_session:
        print("\nFiles in folder but not in session.xml:")
        for file in sorted(files_not_in_session):
            print(f"  {file}")
        # Export mismatches to CSV
        export_mismatches_to_csv(files_not_in_session)
            
    if files_not_in_folder:
        print("\nFiles in session.xml but not in folder:")
        for file in sorted(files_not_in_folder):
            print(f"  {file}")
            
    if not files_not_in_session and not files_not_in_folder:
        print("\nAll files match between session.xml and folder!")

def export_to_csv(files: List[Dict[str, str]], output_file: str = "session_files.csv"):
    """
    Export the session files information to a CSV file.
    
    Args:
        files (List[Dict[str, str]]): List of file information dictionaries
        output_file (str): Name of the output CSV file
    """
    try:
        # Get all unique keys from all file dictionaries
        fieldnames = set()
        for file_info in files:
            fieldnames.update(file_info.keys())
        
        # Remove backup_exists from fieldnames as it's internal tracking
        if 'backup_exists' in fieldnames:
            fieldnames.remove('backup_exists')
    
        #fieldnames = sorted(list(fieldnames))
        
        with open(output_file, 'w', newline='', encoding='utf-8') as csvfile:
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            writer.writeheader()
            
            for file_info in files:
                # Create a new dict without backup_exists
                row = {k: v for k, v in file_info.items() if k != 'backup_exists'}
                writer.writerow(row)
                
        print(f"\nSuccessfully exported file information to {output_file}")
        
    except Exception as e:
        print(f"Error exporting to CSV: {e}")

def main():
    # Prompt for session.xml location
    while True:
        file_path = input("Enter path to session.xml: ").strip()
        if os.path.isfile(file_path):
            break
        print("Invalid file path. Please enter a valid path to session.xml")
    
    files = parse_session_files(file_path)
    
    if files:
        print(f"\nFound {len(files)} files in the session:")
        for i, file_info in enumerate(files, 1):
            print(f"\nFile {i}:")
            for key, value in file_info.items():
                if key == 'backupFilePath':
                    exists_str = "EXISTS" if file_info.get('backup_exists', False) else "MISSING"
                    print(f"{key}: {value} ({exists_str})")
                elif key != 'backup_exists':  # Skip printing the backup_exists flag
                    print(f"{key}: {value}")
        
        # Export to CSV
        export_to_csv(files)
        
        # Get folder path from user
        folder_path = input("\nEnter folder path to compare: ").strip()
        if os.path.isdir(folder_path):
            folder_files = get_folder_files(folder_path)
            print(f"\nFound {len(folder_files)} files in folder")
            compare_files(files, folder_files)
        else:
            print("Invalid folder path!")

if __name__ == "__main__":
    main() 
~~~