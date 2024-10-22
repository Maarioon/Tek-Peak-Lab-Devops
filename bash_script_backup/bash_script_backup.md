This Bash script helps automate backing up a directory and includes functions to display colored messages for enhanced readability. The script prompts the user for directories and a date, validates inputs, and then creates a backup file. Colored echo messages are used to indicate the script's progress and any errors.

## Script Features

- **Purple and Orange Echo Messages**: Colored messages to indicate progress and errors.
- **Input Validation**: Ensures valid directories are provided.
- **Backup Creation**: Compresses the target directory into a `.tar.gz` file and stores it in the destination directory.
- **Status Check**: Confirms whether the backup was successful or not.
![Screenshot 2024-10-18 104143](https://github.com/user-attachments/assets/4d0684bb-30fb-4206-828d-0fba84bf7e61)
![Screenshot 2024-10-18 095858](https://github.com/user-attachments/assets/2ded8751-1305-494f-8743-be034284b599)

## Bash Script

```bash
#!/bin/bash

# Functions to echo parts of the script in color
purple_echo() {
    echo -e "\e[35m$1\e[0m"
}

orange_echo() {
    echo -e "\e[31m$1\e[0m"
}

# Starting the script with a colored message
purple_echo "Script is starting now"

# Asking for user input
read -rp "Enter the path to the directory you want to backup: " target_dir
read -rp "Enter the path to the directory where you want to save the backup: " destination_dir
read -rp "What's today's date? (Format: YYYYMMDD) " day_date

# Validate the directories
if [[ ! -d "$target_dir" ]] || [[ ! -d "$destination_dir" ]]; then
    orange_echo "One (or both) of the paths you entered do not lead to a directory, please revise"
    exit 1
fi

# Define the backup filename and path
backup="backup_${day_date}.tar.gz"
backup_path="${destination_dir}/${backup}"

# Create the backup
tar -cvzf "$backup_path" "$target_dir"

# Notify the user the backup process has started
purple_echo "Now backing up files................................"

# Check if the backup was successful
if [ $? -eq 0 ]; then
    echo "Backup completed successfully. Saved as $backup_path"
else
    echo "Backup failed"
fi

# End of script message
orange_echo "Done with Backup"
```

## Script Breakdown

1. **Colored Echo Functions**:
   - `purple_echo()`: Prints a message in purple (using the `\e[35m` escape code).
   - `orange_echo()`: Prints a message in orange/red (using the `\e[31m` escape code).
   
2. **User Input**:
   - Prompts for the target directory, destination directory, and date.
   
3. **Validation**:
   - Checks if the input paths are valid directories. If not, it prints an error message in orange and exits.

4. **Backup Process**:
   - Compresses the target directory into a `.tar.gz` file and stores it in the destination directory, using the date in the filename.

5. **Progress Messages**:
   - Prints colored messages to indicate when the script starts, when the backup is being created, and when the script ends.

6. **Backup Status**:
   - Checks if the `tar` command succeeds. If successful, it confirms the backup completion. If not, it prints an error message.

## Usage Instructions

1. **Copy the Script**: Copy the above Bash script into a file named `backup_script.sh`.

2. **Make the Script Executable**:

   ```bash
   chmod +x backup_script.sh
   ```

3. **Run the Script**:

   ```bash
   ./backup_script.sh
   ```

4. The script will prompt you for the target directory, destination directory, and the date. Once provided, it will create the backup and print progress and error messages in color.

![Screenshot 2024-10-18 104643](https://github.com/user-attachments/assets/60d9c4f1-9131-4f9f-8df5-4a18e576f201)
