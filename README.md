from pathlib import Path
import shutil

CATEGORIES = {
    "Images": [".jpg", ".jpeg", ".png", ".gif", ".webp"],
    "Documents": [".pdf", ".docx", ".txt", ".xlsx"],
    "Videos": [".mp4", ".mkv", ".mov"],
    "Audio": [".mp3", ".wav", ".flac"],
    "Archives": [".zip", ".rar", ".7z"],
    "Code": [".py", ".js", ".html", ".css"]
}

def get_category(extension):
    for category, extensions in CATEGORIES.items():
        if extension.lower() in extensions:
            return category
    return "Others"

def organize_folder(folder_path):
    folder = Path(folder_path).expanduser()

    if not folder.is_dir():
        print("Invalid folder path!")
        return

    moved = 0

    for file in folder.iterdir():
        if not file.is_file():
            continue

        category = get_category(file.suffix)
        destination = folder / category
        destination.mkdir(exist_ok=True)

        target = destination / file.name

        if target.exists():
            print(f"Skipped existing file: {file.name}")
            continue

        try:
            shutil.move(str(file), str(target))
            print(f"Moved: {file.name} -> {category}")
            moved += 1
        except OSError as error:
            print(f"Could not move {file.name}: {error}")

    print(f"\nOrganization complete! {moved} files moved.")

def main():
    print("=== Python File Organizer ===")
    print("Enter the folder you want to organize.")

    folder_path = input("Folder path: ").strip()

    if folder_path:
        organize_folder(folder_path)
    else:
        print("Please enter a folder path.")

if __name__ == "__main__":
    main()
