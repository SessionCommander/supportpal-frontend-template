# SessionCommander SupportPal Frontend Template

A customized [Twig](https://twig.symfony.com) template for [SupportPal](https://www.supportpal.com) that perfectly matches the SessionCommander website design, creating a seamless user experience.

## Customization Details

This template has been customized to match the SessionCommander website styling:

- **Dark Theme**: Full dark mode with gray-950 background and gray-900 content areas
- **Purple Accents**: Purple gradient buttons and accent colors matching the website (#7e22ce, #9333ea, #a855f7)
- **Typography**: Inter and Playfair Display fonts matching the main website
- **Branding**: SessionCommander logo and branding throughout
- **Header**: Sticky navigation with backdrop blur, matching website header
- **Footer**: Four-column layout with Quick Links and Resources sections
- **Responsive Design**: Mobile-first approach with full responsive support
- **Custom Styling**: All form elements, buttons, and UI components styled to match

## Installation

### Manual Installation

1. Copy all template files to your SupportPal installation:
   ```
   /path/to/supportpal/resources/templates/frontend/sessioncommander/
   ```
   
   **Important:** 
   - The correct path is `resources/templates/frontend/` (note: "templates" with an 's')
   - The template directory name (`sessioncommander`) must not contain any periods
   - Ensure the directory is created if it doesn't exist

2. Log in to your SupportPal admin panel
3. Navigate to **Settings → Core → Brands**
4. Select the brand you wish to set it for
5. Click the "Website" tab
6. Change the Frontend Template to `sessioncommander` (must match the directory name exactly)
7. Click Save

## Automated Deployment

This template includes a GitHub Actions workflow for automated deployment to your server.

### Prerequisites

1. A server with SSH access
2. The SupportPal installation root directory path (e.g., `/var/www/supportpal`)
3. SSH key pair for authentication

### Setup Steps

1. **Generate SSH Key Pair (if you don't have one):**
   ```bash
   ssh-keygen -t ed25519 -C "github-actions-deploy"
   # Save it as ~/.ssh/github_deploy_key (or any name you prefer)
   ```

2. **Add Public Key to Server:**
   ```bash
   # Copy the public key to your server
   ssh-copy-id -i ~/.ssh/github_deploy_key.pub user@your-server.com
   
   # Or manually add to ~/.ssh/authorized_keys on the server
   cat ~/.ssh/github_deploy_key.pub | ssh user@your-server.com "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
   ```

3. **Configure GitHub Secrets:**
   - Go to your repository on GitHub
   - Navigate to Settings → Secrets and variables → Actions
   - Add the following secrets:
     
     **DEPLOY_HOST** (required)
     - Your server's hostname or IP address
     - Example: `example.com` or `192.168.1.100`
     
     **DEPLOY_USER** (required)
     - SSH username for your server
     - Example: `deploy` or `www-data`
     
     **DEPLOY_SSH_KEY** (required)
     - Your private SSH key (the entire contents of the private key file)
     - Copy the entire key including `-----BEGIN OPENSSH PRIVATE KEY-----` and `-----END OPENSSH PRIVATE KEY-----`
     - Example: Get it with `cat ~/.ssh/github_deploy_key`
     
     **DEPLOY_PATH** (required)
     - The SupportPal installation root directory on your server
     - This should be the full path to your SupportPal installation root
     - The template will be deployed to: `$DEPLOY_PATH/resources/templates/frontend/sessioncommander`
     - Note: The path uses `templates` (plural) not `template`
     - The template directory will be created automatically if it doesn't exist
     - Example: `/var/www/supportpal` or `/opt/supportpal`
     
     **DEPLOY_PORT** (optional)
     - SSH port (defaults to 22 if not set)
     - Example: `2222` if using a non-standard port

4. **Deploy:**
   - Push to the `master` branch, or
   - Manually trigger the workflow from Actions → Deploy to Server → Run workflow

### How It Works

1. On push to `master`, GitHub Actions:
   - Checks out your code
   - Ensures the template directory exists at `$DEPLOY_PATH/resources/templates/frontend/sessioncommander`
   - Uses rsync to efficiently sync template files to your server via SSH
   - Deploys to the template directory (removes old files, adds new ones)
   - Clears the SupportPal template cache to ensure new templates are loaded

### Server Requirements

- SSH access enabled
- Write permissions to the SupportPal installation directory
- The template will be deployed to: `resources/templates/frontend/sessioncommander/` within your SupportPal installation
- The template directory name must not contain periods (as per SupportPal requirements)

### Activating the Template in SupportPal

After deployment:

1. Log in to your SupportPal admin panel
2. Navigate to **Settings → Core → Brands**
3. Select the brand you wish to set it for
4. Click the "Website" tab
5. Change the Frontend Template to your new template directory name
6. Click Save

## Troubleshooting

**SSH Connection Fails:**
- Verify the SSH key is correctly added to GitHub Secrets
- Check that the public key is in `~/.ssh/authorized_keys` on the server
- Verify host, username, and port are correct
- Test SSH connection manually: `ssh -i ~/.ssh/github_deploy_key user@host`

**Permission Denied:**
- Ensure the deployment user has write access to the target directory
- Check directory permissions: `chmod 755 /path/to/template`
- Consider using a dedicated deployment user

**Files Not Updating:**
- Check that the deployment is successful in GitHub Actions logs
- Verify the DEPLOY_PATH is set to the SupportPal root directory (not the template directory)
- Ensure the template directory name matches what's configured in SupportPal (`sessioncommander`)
- Check that the cache was cleared successfully

**Template Not Appearing:**
- Verify the template directory name matches exactly (case-sensitive)
- Clear SupportPal cache if available
- Check file permissions on the template directory

## Security Notes

- Never commit your private SSH key to the repository
- Use GitHub Secrets for all sensitive information
- Consider using a dedicated deployment user with limited permissions
- Regularly rotate SSH keys

## Related Resources

- [SupportPal Documentation](https://docs.supportpal.com/current/Templates)
- [Twig Documentation](https://twig.symfony.com/docs)
- [SupportPal Official Templates](https://github.com/supportpal/frontend-template)
