# Automated Web Security Scanner

A beginner-friendly Python tool that audits websites for missing **HTTP security headers** and **CORS misconfigurations**, then gives each site a score (0-100), a grade (A-F), charts and a CSV report.

Built as the project submission for the **IBM SkillsBuild National Cybersecurity Bootcamp** (BharatCares, 5-19 October 2026).

**Author:** YashJangir  
**Project idea:** #2 - Automated Web Scanner

---

## Project Description

Many web attacks (XSS, clickjacking, protocol downgrade, data theft through cross-origin requests) can be reduced by sending the right security headers and configuring CORS correctly. Checking these by hand for every site is slow, so this project automates it.

For every URL in a list, the scanner:

1. Sends a normal `GET` request.
2. Checks 6 security headers:

   | Header | Points | Protects against |
   |---|---|---|
   | Content-Security-Policy | 25 | XSS / script injection |
   | Strict-Transport-Security | 20 | HTTPS downgrade attacks |
   | X-Frame-Options (or CSP `frame-ancestors`) | 15 | Clickjacking |
   | X-Content-Type-Options (`nosniff`) | 10 | MIME sniffing |
   | Referrer-Policy | 10 | URL information leaks |
   | Permissions-Policy | 10 | Misuse of camera, mic, location |

3. Runs a **CORS test** by sending fake `Origin` headers (`https://evil-attacker.example` and `null`) and checking if the server trusts them (10 points).
4. Calculates a **score out of 100** and a **grade** (A >= 90, B >= 75, C >= 60, D >= 40, F below 40).
5. Produces a results table, two charts, fix recommendations and a CSV file.

## Dataset

**No external dataset is used.** The tool scans live URLs provided by the user in the `TARGETS` list inside the notebook.

## Technologies Used

- Python 3.9+
- `requests` - sending HTTP requests and reading headers
- `pandas` - results table and CSV export
- `matplotlib` - charts
- Jupyter Notebook

## Setup and Run Instructions

```bash
# 1. (Optional) create a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Linux / macOS

# 2. Install dependencies
pip install -r requirements.txt

# 3. Start Jupyter and open the notebook
jupyter notebook YashJangir_AutomatedWebScanner.ipynb
```

4. Edit the `TARGETS` list in the **Configuration** cell (only use sites you own or that are meant for testing).
5. Click **Kernel -> Restart & Run All**.

An internet connection is required. The full scan of 5-6 sites takes under a minute and uses very little memory (runs fine on 8 GB RAM).

## Output Files

| File | Description |
|---|---|
| `scan_results.csv` | Full results for every site |
| `security_scores.png` | Bar chart of score per site |
| `missing_headers.png` | Chart of the most commonly missing headers |

## Key Information

- **Ethical use:** only scan websites you own or have permission to test. The tool sends only normal GET requests - no exploitation, brute force or payloads.
- **Limitations:** checks only the main page of each site, and only the header *presence* (it does not judge how strong a CSP policy is).
- **Future scope:** cookie flag checks (`Secure`, `HttpOnly`, `SameSite`), TLS certificate checks, multi-page crawling, HTML/PDF report export.

## Project Structure

```
YashJangir_AutomatedWebScanner.ipynb   # main project code
requirements.txt                     # dependencies
YashJangir_ProjectReport.docx          # full project report
README.md                            # this file
```
