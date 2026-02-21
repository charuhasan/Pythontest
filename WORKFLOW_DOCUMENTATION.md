# GitHub Actions Workflow Documentation

## SBOM Scan and GitHub Insights Workflow

This workflow automatically scans Docker images for vulnerabilities and generates Software Bill of Materials (SBOM) reports, submitting them to GitHub Insights.

### What It Does

1. **Builds Docker Image** - Creates a Docker image from the Dockerfile
2. **Vulnerability Scanning** - Uses Trivy to scan for security vulnerabilities
3. **SBOM Generation** - Creates both SPDX and CycloneDX format SBOMs
4. **GitHub Integration** - Uploads results to GitHub Security tab and Dependency Graph
5. **Reporting** - Generates artifacts and PR comments with detailed reports

### Triggers

The workflow runs:
- **On Push**: When Dockerfile, requirements.txt, or app.py changes on main/develop branches
- **On Pull Request**: When these files change in PRs to main
- **Scheduled**: Daily at 2 AM UTC
- **Manual**: Can be triggered manually via GitHub Actions UI

### Features

#### 1. Trivy Security Scanning
- Scans Docker image for CVEs and vulnerabilities
- Uploads results to GitHub Security tab (Code scanning alerts)
- SARIF format for integration with GitHub's security features

#### 2. SBOM Generation
- **SPDX Format**: Industry-standard Software Bill of Materials
- **CycloneDX Format**: Popular format for software composition analysis
- Lists all dependencies and their versions

#### 3. GitHub Insights Integration
- **Dependency Graph**: Automatically updates when pushed to default branch
- **Dependency Alerts**: GitHub can notify about vulnerable dependencies
- **Supply Chain Security**: Part of GitHub's security ecosystem

#### 4. Artifact Management
- Stores SBOM and scan reports for 90 days
- Available in Actions artifacts for download
- Can be used for compliance and auditing

#### 5. PR Comments
- Automatically comments on PRs with SBOM summary
- Shows component count and artifact locations

### Workflow Permissions

The workflow requires:
- `contents: write` - To upload artifacts and update dependency graph
- `security-events: write` - To upload security scan results
- `packages: read` - To read Docker image information

### How to Use

#### 1. Enable in Repository
Push the `.github/workflows/sbom-scan.yml` file to your repository.

#### 2. Manual Trigger
1. Go to **Actions** tab in GitHub
2. Select **Docker SBOM Scan and Insights** workflow
3. Click **Run workflow**

#### 3. View Results

**Vulnerabilities**:
- Go to **Security** > **Code scanning alerts** tab

**Dependency Information**:
- Go to **Insights** > **Dependency graph** tab
- Check **Dependabot alerts** if enabled

**Artifacts**:
- Go to **Actions** > Run details > **Artifacts** section
- Download SBOM reports

### Customization

#### Change Scan Triggers
Edit the `on:` section to customize when the workflow runs:

```yaml
on:
  push:
    branches:
      - main
  # Add other triggers as needed
```

#### Modify Schedule
Change the cron expression (default: `0 2 * * *` = 2 AM UTC daily):

```yaml
schedule:
  - cron: '0 2 * * *'  # Format: minute hour day month day-of-week
```

#### Include Additional Files in Scan
Add paths to the trigger:

```yaml
paths:
  - 'Dockerfile'
  - 'requirements.txt'
  - 'app.py'
  - 'src/**'  # Scan entire src directory
```

### Tools Used

- **Trivy** (aquasecurity): Comprehensive vulnerability scanner for containers
- **Syft** (anchore): SBOM generation tool
- **GitHub Dependency Submission Action**: Uploads SBOMs to dependency graph
- **GitHub CodeQL**: Security analysis integration

### Security Best Practices

1. **Regular Scans**: Workflow runs automatically and on push
2. **PR Checks**: Validates changes before merging
3. **Artifact Retention**: Keeps reports for compliance
4. **Supply Chain Security**: Tracks all dependencies

### Troubleshooting

#### Workflow Fails to Run
- Verify `.github/workflows/sbom-scan.yml` is in your repository
- Check GitHub Actions are enabled (Settings > Actions)

#### No Results Appearing
- Wait for workflow to complete (visible in Actions tab)
- Check if "push to default branch" requirement is met for dependency graph
- Verify repository visibility (some features require public repo)

#### SBOM Not Appearing in Dependency Graph
- Only updates when push to default branch (main)
- Requires `contents: write` permission
- May take a few moments to appear

### Example Output

The workflow generates:
- `sbom-spdx.json` - SPDX format SBOM
- `sbom-cyclonedx.json` - CycloneDX format SBOM
- `trivy-results.sarif` - Security scan results
- **Artifacts**: All files available in Actions for download
- **PR Comments**: Component count and links (on pull requests)
- **GitHub Insights**: Updates dependency information

### Next Steps

1. Commit and push the workflow file
2. Go to Actions tab to monitor first run
3. Check Security tab for vulnerabilities
4. Review Dependency Graph in Insights
5. Configure Dependabot alerts if desired
