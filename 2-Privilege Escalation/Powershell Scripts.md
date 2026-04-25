# Powershell Scripts

## 1. Create a New Local User Account

```
net user attackerlab P@ssw0rd123 /add
```

## 2. Add User to Administrators Group

```
net localgroup administrators attackerlab /add
```

## 3. (Optional) - Delete User After Completion

```
net user attackerlab /delete
```

## 4. Verify User Delete

```
net user
```
Username of Privileged account should now no longer appear.
