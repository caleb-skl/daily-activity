# Daily Activity Bot 🚀

An automated, serverless GitHub Action designed to keep repository activity fresh by generating daily timestamped commits without requiring third-party hosting, external servers, or cloud credentials.

---

## 📌 How It Works

This repository uses **GitHub Actions** and native cron schedules:

1. **Trigger:** A scheduled workflow (`.github/workflows/auto-commit.yml`) runs automatically every day at `00:00 UTC` (or via manual trigger).
2. **Execution:** An Ubuntu runner checks out the repo and writes the latest UTC timestamp into `log.txt`.
3. **Commit & Push:** The runner stages `log.txt`, commits the change, and pushes directly back to the `main` branch.

---

## 🛠️ Repository Structure

```text
daily-activity/
├── .github/
│   └── workflows/
│       └── auto-commit.yml   # Workflow configuration & cron scheduler
├── log.txt                   # Auto-updated record containing the last run timestamp
└── README.md                 # Project documentation

```

---

## ⚙️ Setup & Configuration

If you fork or replicate this repository, follow these steps to enable automated updates:

### 1. Workflow Permissions

GitHub Actions requires write access to push commits back to your branch:

1. Go to **Settings** > **Actions** > **General**.
2. Under **Workflow permissions**, select **Read and write permissions**.
3. Click **Save**.

### 2. Manual Run (Testing)

You can test the action at any time without waiting for the scheduled cron:

1. Navigate to the **Actions** tab.
2. Select **Daily Commit** from the left sidebar.
3. Click **Run workflow** > select branch `main` > click **Run workflow**.

---

## 🔧 Customization

### Changing the Schedule

The cron schedule can be adjusted inside `.github/workflows/auto-commit.yml`:

```yaml
on:
  schedule:
    - cron: '0 12 * * *' # Format: Minute Hour Day Month Day-of-Week (UTC)

```

### Linking Commits to Your Profile

By default, the workflow commits as `github-actions[bot]`. To attribute commits to your personal GitHub activity graph, update the Git identity step in the workflow to match your GitHub credentials:

```yaml
- name: Commit and push changes
  run: |
    git config --global user.name "YourUsername"
    git config --global user.email "your-email@users.noreply.github.com"
    git add log.txt
    git commit -m "chore: automated daily update [skip ci]" || exit 0
    git push

```

> **Note:** If your repository is set to private, ensure you check **"Include private contributions on my profile"** in your GitHub Profile settings.

---

## 📄 License

This project is open-source and available under the MIT License.

```

```
