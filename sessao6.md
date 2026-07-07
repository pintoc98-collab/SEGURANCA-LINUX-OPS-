# CTF Session 6 – Linux Administration Challenge

## Objective

The goal of this CTF was to solve four Linux administration tasks involving:

- Log analysis
- File and directory permissions
- Process management
- Backup creation

---

# Problem 1 – Log Analysis

### My first attempt

I initially counted the total number of lines in the log file using:

```bash
wc -l logs/sistema.log
```

Output:

```text
7 logs/sistema.log
```

Since I thought the task only required counting lines, I redirected the output into the report file:

```bash
wc -l logs/sistema.log > relatorio_logs.txt
```

After running the checker, Problem 1 was still marked as **unsolved**.

Looking at the contents of the report:

```bash
cat relatorio_logs.txt
```

I realized it contained:

```text
7 logs/sistema.log
```

The challenge wasn't asking for the total number of lines—it wanted the number of lines containing the word **ERRO**.

### Solution

I filtered only the error lines before counting them:

```bash
grep "ERRO" logs/sistema.log | wc -l > relatorio_logs.txt
```

Running the checker again confirmed that Problem 1 was solved.

---

# Problem 2 – File Permissions

I navigated into the `financas` directory:

```bash
cd financas
```

and changed the permissions of the confidential file:

```bash
chmod 750 relatorio_confidencial.txt
```

To verify the permissions I used:

```bash
ls -la
```

and also:

```bash
stat -c "%a" relatorio_confidencial.txt
```

which correctly returned:

```text
750
```

However, the checker still failed.

Reading the checker output more carefully, I noticed it also expected the **directory itself** to have permission `750`, not only the file.

I fixed both at once:

```bash
chmod 750 financas
chmod 750 financas/relatorio_confidencial.txt
```

After rerunning the verification script, the permissions challenge passed.

---

# Problem 3 – Process Management

To locate the background `sleep` process I first tried:

```bash
pgrep sleep
```

It returned:

```text
3892
```

I attempted to terminate it:

```bash
kill 3892
```

but received:

```text
No such process
```

I also mistakenly tried:

```bash
pkill -9 3892
```

which didn't solve the challenge because `pkill` expects a process **name**, not a PID.

After running the checker again, it explicitly indicated that the actual process was:

```text
PID 570
```

I terminated the correct process:

```bash
kill -9 570
```

The next verification confirmed the process challenge was complete.

---

# Problem 4 – Backup Creation

At first I simply copied the directory:

```bash
cp -r financas ./backup_pendente/
```

Although this created a backup folder, the checker still failed.

Reading the hint showed that the challenge required a compressed archive (`.tar.gz`), not just a copied directory.

I created the archive with:

```bash
tar -czf backup_pendente/backup_financas.tar.gz financas/
```

Running the checker afterwards confirmed the backup requirement was satisfied.

---

# Final Result

After correcting each mistake and learning from the checker hints, all four challenges were successfully completed.

```text
🏆 PARABÉNS! Servidor CVTECH recuperado!

Flag:
CVTECH{log5_perm1ssoes_process0_segur4nca}
```

---

<img width="1832" height="1312" alt="image" src="https://github.com/user-attachments/assets/9a248470-08d6-4dce-9b1b-743af4dffd3b" />


# What I Learned

This exercise reinforced several important Linux administration concepts:

- Using `grep` together with `wc` to analyze log files.
- Redirecting command output into files with `>`.
- Understanding Linux file and directory permissions using `chmod`.
- Verifying permissions with both `ls -l` and `stat`.
- Finding and terminating running processes using `pgrep`, `ps`, and `kill`.
- Understanding the difference between `kill` (PID) and `pkill` (process name).
- Creating compressed backups using `tar`.

One of the most valuable lessons was learning to read the verification output carefully. Instead of guessing, I used the hints and error messages to identify what the challenge was actually expecting, corrected my approach, and eventually completed every task successfully.
