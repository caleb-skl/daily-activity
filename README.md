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
├── LICENSE                   # MIT License
└── README.md                 # Project documentation

```

---

## ⚙️ Setup & Configuration

If you fork or clone this repository, complete these steps to enable automated updates:

### 1. Workflow Permissions

GitHub Actions requires write access to push commits back to your repository:

1. Go to **Settings** > **Actions** > **General**.
2. Under **Workflow permissions**, select **Read and write permissions**.
3. Click **Save**.

### 2. Update Username & Email (Required for Green Boxes)

By default, the default GitHub Actions bot (`github-actions[bot]`) does **not** count toward your personal contribution graph. You must configure the Git identity to match your GitHub profile:

1. Open `.github/workflows/auto-commit.yml`.
2. Find the `Commit and push changes` step.
3. Update `user.name` and `user.email` with your credentials:

```yaml
- name: Commit and push changes
  run: |
    git config --global user.name "YOUR_GITHUB_USERNAME"
    git config --global user.email "YOUR_EMAIL_OR_NOREPLY_ADDRESS"
    git add log.txt
    git commit -m "chore: automated daily update [skip ci]" || exit 0
    git push

```

> 🔒 **Protect Your Privacy:** If you do not want your personal email address visible in public commit history, use your GitHub-provided noreply address:
> 1. Go to GitHub **Settings** > **Emails**.
> 2. Check **Keep my email addresses private**.
> 3. Copy the address shown under that section (formatted as `ID+username@users.noreply.github.com`) and paste it into `git config --global user.email`.
> 
> 

### 3. Manual Run (Testing)

Verify that your workflow runs correctly without waiting for the scheduled cron:

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

> **Note for Private Repositories:** If your repository is set to private, ensure you check **"Include private contributions on my profile"** under **Contribution settings** on your GitHub profile page.

---

## 📄 License

This project is open-source and available under the MIT License.
