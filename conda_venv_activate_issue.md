# Conda Environment Activation Fix (PowerShell + VS Code)

## Problem

The Conda environment existed and was valid, but PowerShell was not loading the Conda hook automatically. Because of this, the command:

```powershell
conda activate .\venv
```

did not activate the environment.

---

## Root Cause

PowerShell was not loading the Conda initialization script (`conda-hook.ps1`) when starting.

Evidence:

```powershell
Get-Command conda
```

Output:

```text
Application conda.bat
```

instead of:

```text
Alias conda -> Invoke-Conda
```

or

```text
Function conda
```

Also, the PowerShell profile did not exist:

```powershell
$PROFILE
Test-Path $PROFILE
```

Output:

```text
False
```

---

# Temporary Fix

Use these steps whenever Conda activation is not working.

### Step 1: Open VS Code PowerShell Terminal

```powershell
cd E:\Data_Analyst_BootCamp\Python
```

### Step 2: Load the Conda PowerShell Hook

```powershell
& "C:\Users\Dell\anaconda3\shell\condabin\conda-hook.ps1"
```

### Step 3: Activate the Environment

```powershell
conda activate E:\Data_Analyst_BootCamp\Python\venv
```

or

```powershell
conda activate .\venv
```

### Step 4: Verify

The prompt should change to:

```text
(E:\Data_Analyst_BootCamp\Python\venv) PS E:\Data_Analyst_BootCamp\Python>
```

---

# Permanent Fix

## Step 1: Create a PowerShell Profile

```powershell
New-Item -ItemType File -Path $PROFILE -Force
```

## Step 2: Open the Profile

```powershell
notepad $PROFILE
```

## Step 3: Add the Conda Hook

Paste the following line into the profile:

```powershell
& "C:\Users\Dell\anaconda3\shell\condabin\conda-hook.ps1"
```

The profile should contain:

```powershell
& "C:\Users\Dell\anaconda3\shell\condabin\conda-hook.ps1"
```

## Step 4: Save and Close Notepad

## Step 5: Restart VS Code

Close all VS Code windows and reopen VS Code.

## Step 6: Verify

Open a new PowerShell terminal and run:

```powershell
Get-Command conda
```

Expected output:

```text
Alias conda -> Invoke-Conda
```

or

```text
Function conda
```

## Step 7: Activate Normally

```powershell
conda activate .\venv
```

No need to run `conda-hook.ps1` manually anymore.

---

# Execution Policy Fix

To allow PowerShell to run the Conda startup script:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Verify:

```powershell
Get-ExecutionPolicy -Scope CurrentUser
```

Expected output:

```text
RemoteSigned
```

This only needs to be done once.

---

# Daily Usage

After applying the permanent fix:

```powershell
cd E:\Data_Analyst_BootCamp\Python
conda activate .\venv
```

Expected prompt:

```text
(E:\Data_Analyst_BootCamp\Python\venv) PS E:\Data_Analyst_BootCamp\Python>
```

Your Conda environment is now ready for use in VS Code.
