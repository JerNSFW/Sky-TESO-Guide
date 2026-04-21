# Termux Setup Guide

## Overview
This guide walks you through setting up Termux for Discord bot development and general Python/JavaScript programming. Termux is an Android terminal emulator that provides a Linux environment.

## Prerequisites
- Android device (Android 6.0 or higher recommended)
- Termux app (download from F-Droid or Google Play Store)
- Minimum 500MB free storage space

## Step 1: Initial Setup & Update

### 1.1 Update Package Manager
```bash
apt update
apt upgrade -y
```

This updates the package lists and upgrades all installed packages to their latest versions.

### 1.2 Install Essential Build Tools
```bash
apt install -y build-essential clang pkg-config
```

## Step 2: Connect to Storage

### 2.1 Request Storage Access
```bash
termux-setup-storage
```

This will prompt you to grant storage permissions. Accept the permission request on your device.

### 2.2 Access Storage Directories
After setup, you can access storage at:
```bash
# Downloads folder
cd ~/storage/downloads

# Documents folder
cd ~/storage/documents

# Home directory (local to Termux)
cd ~/
```

### 2.3 Create Working Directory
```bash
mkdir -p ~/storage/downloads/discord-bot
cd ~/storage/downloads/discord-bot
```

## Step 3: Install Python

### 3.1 Install Python
```bash
apt install -y python python-pip
```

### 3.2 Verify Installation
```bash
python --version
pip --version
```

### 3.3 Upgrade pip
```bash
pip install --upgrade pip
```

### 3.4 Install Common Python Packages
```bash
pip install requests urllib3 python-dotenv
```

## Step 4: Install JSON Support

### 4.1 JSON is Built-in to Python
JSON support comes pre-installed with Python. Verify with:
```bash
python -c "import json; print('JSON support available')"
```

### 4.2 Install Additional JSON Tools (Optional)
```bash
apt install -y jq  # Command-line JSON processor
pip install jsonschema ujson  # Advanced JSON libraries
```

## Step 5: Install JavaScript/Node.js

### 5.1 Install Node.js and npm
```bash
apt install -y nodejs npm
```

### 5.2 Verify Installation
```bash
node --version
npm --version
```

### 5.3 Initialize Node Project (Optional)
```bash
cd ~/storage/downloads/discord-bot
npm init -y
```

## Step 6: Install Discord Bot Dependencies

### 6.1 For Python Discord Bot

#### Option A: discord.py (Recommended)
```bash
pip install discord.py
```

#### Option B: interactions.py (Slash Commands)
```bash
pip install discord-py-interactions
```

#### Option C: py-cord
```bash
pip install py-cord
```

### 6.2 For JavaScript Discord Bot

#### Install discord.js
```bash
npm install discord.js
```

#### Install Additional Packages
```bash
npm install dotenv axios
```

### 6.3 Install ffmpeg for Audio (Optional but Recommended)
```bash
apt install -y ffmpeg
```

This is required for playing audio in Discord voice channels.

## Step 7: Environment Setup

### 7.1 Create .env File
```bash
# Navigate to your project directory
cd ~/storage/downloads/discord-bot

# Create environment file
nano .env
```

Add your Discord token:
```
DISCORD_TOKEN=your_bot_token_here
```

Press `Ctrl+X`, then `Y`, then `Enter` to save.

### 7.2 Make .env Readable by Your Bot
```bash
chmod 600 .env
```

## Step 8: Create Sample Bot Files

### 8.1 Python Discord Bot Template
```bash
nano bot.py
```

Paste this code:
```python
import discord
from discord.ext import commands
import os
from dotenv import load_dotenv

load_dotenv()
TOKEN = os.getenv('DISCORD_TOKEN')

bot = commands.Bot(command_prefix='!', intents=discord.Intents.default())

@bot.event
async def on_ready():
    print(f'{bot.user} has connected to Discord!')

@bot.command(name='ping')
async def ping(ctx):
    await ctx.send('Pong!')

bot.run(TOKEN)
```

### 8.2 JavaScript Discord Bot Template
```bash
nano bot.js
```

Paste this code:
```javascript
const { Client, Intents } = require('discord.js');
const client = new Client({ intents: [Intents.FLAGS.GUILDS, Intents.FLAGS.GUILD_MESSAGES] });
require('dotenv').config();

client.on('ready', () => {
  console.log(`${client.user.tag} has connected to Discord!`);
});

client.on('messageCreate', message => {
  if (message.content === '!ping') {
    message.reply('Pong!');
  }
});

client.login(process.env.DISCORD_TOKEN);
```

## Step 9: Running Your Bot

### 9.1 Python Bot
```bash
python bot.py
```

### 9.2 JavaScript Bot
```bash
node bot.js
```

## Troubleshooting

### Issue: "Module not found"
**Solution:** Ensure you've installed the package:
```bash
pip install <package_name>  # For Python
npm install <package_name>  # For JavaScript
```

### Issue: "Permission denied" errors
**Solution:** Grant storage permission:
```bash
termux-setup-storage
```

### Issue: Python/Node commands not found
**Solution:** Verify installation:
```bash
which python
which node
```

If not found, reinstall:
```bash
apt install -y python nodejs
```

### Issue: Bot won't connect
**Solution:** 
1. Verify your token in `.env` is correct
2. Check your internet connection
3. Ensure bot has proper permissions in Discord server settings

## Useful Commands

```bash
# Update everything
apt update && apt upgrade -y

# List installed packages
apt list --installed

# Search for packages
apt search <package_name>

# Remove package
apt remove -y <package_name>

# Check storage space
df -h

# List files in directory
ls -la

# Change directory permissions
chmod 755 directory_name

# Create directory
mkdir -p directory/path
```

## Performance Tips

1. **Close unnecessary apps** to free up RAM
2. **Use lighter Discord libraries** if experiencing lag
3. **Enable power saving** if bot runs slowly
4. **Use virtual environment for Python:**
   ```bash
   pip install virtualenv
   virtualenv env
   source env/bin/activate
   ```

## Security Best Practices

1. **Never hardcode tokens** - Use `.env` files
2. **Protect your bot token** - Regenerate if exposed
3. **Set proper file permissions** - Use `chmod 600` for sensitive files
4. **Keep packages updated** - Run `apt upgrade` regularly

## Next Steps

1. Create your Discord bot on [Discord Developer Portal](https://discord.com/developers/applications)
2. Invite bot to your server
3. Add your bot token to `.env` file
4. Run your bot and start developing!

## Resources

- [Discord.py Documentation](https://discordpy.readthedocs.io/)
- [Discord.js Documentation](https://discord.js.org/)
- [Termux Wiki](https://wiki.termux.com/)
- [Discord Developer Documentation](https://discord.com/developers/docs)

---

Last updated: 2026-04-21