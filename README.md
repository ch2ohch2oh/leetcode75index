# LeetCode 75 Index

A monitoring system that tracks online user activity across all 75 problems in the LeetCode 75 study plan. The index aggregates the total number of users currently viewing these problems.

## 🚀 Live Demo

View the live dashboard: `https://YOUR_USERNAME.github.io/YOUR_REPO/`

## 📊 Features

- **Automated data collection** via GitHub Actions (runs hourly)
- **Interactive dashboard** with time-series visualization
- **GitHub Pages deployment** - no server required
- **EST timezone** for consistent data recording

## Scripts

### 1. `leetcode_client.py`

Handles WebSocket connections to LeetCode's collaboration service.

Usage:
```bash
./leetcode_client.py
# Or with specific inputs
./leetcode_client.py -i custom_problems.txt
# View help
./leetcode_client.py --help
```

### 2. `collect_stats.py`
Periodic data collection script that aggregates total online users across configured indices (e.g., LeetCode 75) and logs results to CSV files.

**Usage:**
```bash
# Run with default config (config/indices.yaml)
./collect_stats.py

# Run with verbose output
./collect_stats.py -v

# Use custom config file
./collect_stats.py -c config/my_indices.yaml

# View help
./collect_stats.py --help
```

**Output Format:**
The CSV file contains the following columns:
- `timestamp`: Date and time of data collection
- `total_users`: Sum of online users across all problems
- `problems_checked`: Number of problems successfully checked
- `problems_failed`: Number of problems that failed to check
- `total_problems`: Total number of problems in the input file

### 3. Fetching Study Plans
Pass `--fetch-plan` to `leetcode_client.py` to retrieve problem lists from LeetCode.

**Usage:**
```bash
# Fetch LeetCode 75 (default)
./leetcode_client.py --fetch-plan leetcode-75

# Fetch another study plan and save to file
./leetcode_client.py --fetch-plan top-interview-150 -o data/top_150.txt
```

## 🚀 Deployment Options

### Option 1: GCP VM (Recommended) ⭐

Deploy on a **free GCP e2-micro VM** with automated hourly cron job:

**📖 See [GCP_DEPLOYMENT.md](GCP_DEPLOYMENT.md) for complete setup guide.**

Quick start:
1. Create GitHub Personal Access Token
2. Edit `deploy-gcp.sh` with your project ID and token
3. Run `./deploy-gcp.sh`
4. VM will collect data every hour and push to GitHub

**Benefits:**
- ✅ 100% free (GCP free tier)
- ✅ Reliable (avoids Cloudflare blocking)
- ✅ Automated (runs 24/7)
- ✅ Simple setup (one script)

### Option 2: GitHub Pages (Manual/Local)

**Note:** GitHub Actions may be blocked by Cloudflare. Use GCP VM for reliable automation.

Quick start:
1. Push this repository to GitHub
2. Enable GitHub Pages (Settings → Pages → Source: GitHub Actions)
3. Your dashboard will be live at `https://YOUR_USERNAME.github.io/YOUR_REPO/`

## Local Development Setup

1. **Create virtual environment:**
```bash
python3 -m venv venv
source venv/bin/activate
```

2. **Install dependencies:**
```bash
pip install -r requirements.txt
```

3. **Make scripts executable:**
```bash
chmod +x leetcode_client.py collect_stats.py generate_site.py
```

## Scheduling Periodic Collection

To automatically collect statistics at regular intervals, use cron:

```bash
# Edit crontab
crontab -e

# Add one of these lines:

# Run every 15 minutes
*/15 * * * * cd /Users/dazhi/code/lcmonitor && source venv/bin/activate && ./collect_stats.py

# Run every hour at minute 0
0 * * * * cd /Users/dazhi/code/lcmonitor && source venv/bin/activate && ./collect_stats.py

# Run every day at 9 AM
0 9 * * * cd /Users/dazhi/code/lcmonitor && source venv/bin/activate && ./collect_stats.py
```

**Note:** Make sure to use absolute paths in cron jobs and activate the virtual environment.

## Data Files

- `data/leetcode75_problems.txt`: List of LeetCode 75 problem slugs (one per line)
- `data/leetcode75_stats.csv`: Historical statistics (created by `collect_stats.py`)

- `site/index.html`: Dashboard visualization

- Python 3.7+
- `requests>=2.31.0`
- `websocket-client>=1.7.0`

## Example Output

### Individual Problem Monitoring
```
$ ./leetcode_client.py
Problem: two-sum                                            | Online Users: 2341
Problem: add-two-numbers                                    | Online Users: 154397
Problem: container-with-most-water                          | Online Users: 360
...
```

### Aggregate Statistics Collection
```
$ ./collect_lc75_stats.py -v
Checking 75 problems...
  [1/75] merge-strings-alternately: 124 users
  [2/75] greatest-common-divisor-of-strings: 103 users
  ...
  [75/75] online-stock-span: 52 users

✓ Results saved to /Users/dazhi/code/lcmonitor/data/lc75_stats.csv
  Timestamp: 2026-01-29 21:11:13
  Total online users: 5706
  Problems checked: 75/75
```

### CSV Output
```csv
timestamp,total_users,problems_checked,problems_failed,total_problems
2026-01-29 21:11:13.608232,5706,75,0,75
2026-01-29 21:12:25.207147,5736,75,0,75
```

## License

MIT

## Visualization

The `site/index.html` file provides a dashboard visualization of the collected data. This file is generated based on your configuration.


**Generate Dashboard:**
```bash
./venv/bin/python generate_site.py
```
This updates `site/index.html`.

**Deployment:**
The site is automatically deployed to GitHub Pages via the workflow in `.github/workflows/deploy.yml`. The build artifact is in the `public/` directory (not committed).
