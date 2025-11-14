# Windows-Automation-Scripts-File-Cleanup
Documenting my experience trying to learn and create an interactive script to clean up duplicate cover letter PDFs from Downloads folder.
# Windows Automation Homelab

## Projects

### Automated File Cleanup (PowerShell)
I wanted to try my hand at automating a task on my computer with the help of scripting and automation. As someone who hardly ever goes back to delete small amounts of files that build up in my downloads folder, I figured a perfect way to skip the hassle of browsing my folder and going through each file, would be to future proof my self and create a script which can identify the files and remove them with a click of a button.
First I used the syntax: ls C:\Users\User\Downloads\*letter*.pdf 
I then discovered the difference between using $env:USERPROFILE instead of the traditional "ls C:\Users" and listing your filepath specifically, which allows this script to be universal and practical for anyone's particular use cases. For me personally I have created a lot of tailored cover letters for job listings and figured this would be handy.

![letter](https://github.com/user-attachments/assets/a167986d-7c13-494d-be64-8f4c3253302c)
Before creating the script I figured I should verify all the files within PowerShell, using the syntax: Get-ChildItem "$env:USERPROFILE\Downloads\*letter*.pdf 

I've written a very simple script in notepad and saved it as a .ps1 file (PowerShell script) This will not confirm your files or prompt you at all but simply run the input Remove-Item (which will delete any pdf file in my downloads folder with "letter" anywhere in its filename) And then display a popup telling you the cover letter files were deleted.
<img width="1090" height="611" alt="image" src="https://github.com/user-attachments/assets/7060ba22-4d92-4277-b95c-1d5f12bb96db" />
<img width="143" height="138" alt="image" src="https://github.com/user-attachments/assets/128e5f50-242e-470b-8bae-a3ecf0ce1b47" />
To run this command simply right click the file and use "Run with PowerShell"
<img width="1213" height="824" alt="image" src="https://github.com/user-attachments/assets/746c370c-2a2c-4436-9d42-31a7f6eec098" />

**Code:** 
Remove-Item "$env:USERPROFILE\Downloads\*letter*.pdf"
Write-Host "Cover letters deleted!"
pause

Copy and paste the above syntax into a notepad document, go to File > Save As > Add a filename with .ps1 at the end, and change the "Save as type:" from text documents to All files.

To search different folders, replace `Downloads` with your target directory
You can also change what file name term you want to delete by just replacing "letter" with your own words. "$env:USERPROFILE\Downloads\*yourterm*.pdf"

## Fileclean up script that will prompt you before removing files

This next script is more indepth and still a bit out of my wheelhouse but the concepts are starting to become more straight forward to me and I'm getting a better grasp of what the syntax is actually doing. This is the syntax if you want to copy and paste it and create your own .ps1 file for yourself, I replaced "letter" for "cover" but it is still in my downloads directory. Feel free to modify it for yourself for your own use cases.

Find PDF files with "cover" in the filename
$files = Get-ChildItem "$env:USERPROFILE\Downloads\*cover*.pdf"

if ($files.Count -eq 0) {
    Write-Host "No matching files found."
} else {
    Write-Host "Found $($files.Count) file(s):"
    $files | ForEach-Object { Write-Host "  - $($_.Name)" }
    
    $confirmation = Read-Host "`nDelete these files? (yes/no)"
    
    if ($confirmation -eq "yes") {
        Remove-Item "$env:USERPROFILE\Downloads\*cover*.pdf"
        Write-Host "Files deleted!"
    } else {
        Write-Host "Cancelled."
    }
}

pause

This script will first check for the files using the Get-ChildItem command. Once it checks for those files it will use the statement "if" and the condition of "if files = 0" the shell will write back to you telling you "No matching files found."
the "else" command in this scenario means "anything other then 0" We specificed what to do if there are 0 files found, which will tell us "No matching files found." but if theres any number above 0, the else command is basically saying otherwise or anything else.



It will then write back to you telling you how many files were found, we will get a question from Read-Host ""`nDelete these files? (yes/no)"
<img width="551" height="380" alt="image" src="https://github.com/user-attachments/assets/3bbf3130-f0ec-4802-b534-44decdc58385" />


the answer will be stored in the $confirmation variable and depending on your answer it will either write back saying "cancelled" or run the command "Remove-Item" and instruct us the files were deleted.
<img width="507" height="467" alt="image" src="https://github.com/user-attachments/assets/659f9618-0d79-4d2d-99d7-e5ad2f8ed55c" />




**Challenges faced:**
- Understanding PowerShell syntax vs CMD
- Learning environment variables (`$env:USERPROFILE`)
- Implementing user confirmation prompts

**Key learnings:**
- PowerShell pipe operators (`|`)
- Conditional logic with `if/else`
- Difference between `.bat` and `.ps1` files
- why environment variables ($env:) are important for creating scripts that can be shared for other people

**Code:** [DeleteCoverLetters.ps1](./powershell/DeleteCoverLetters.ps1)

---

...
