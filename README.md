# HugChat Issue Bot 🤖

A GitHub Action that automatically responds to issues using HuggingFace HugChat with **persistent conversation memory**. Each GitHub issue maintains its own continuous conversation thread, allowing for contextual follow-up questions and responses.

## ✨ Features

- 🧠 **Persistent Conversations**: Each GitHub issue gets its own HugChat conversation that remembers context across multiple comments
- 🔄 **Automatic Session Management**: Finds existing conversations or creates new ones automatically
- 🧹 **Auto Cleanup**: Automatically deletes HugChat conversations when issues are closed
- 🔍 **Web Search**: Optionally enables web search for more accurate responses
- 🎯 **Custom Assistants**: Use your own HuggingFace assistants for specialized responses
- 📝 **Smart Context**: Handles both issue titles and descriptions intelligently

## 🚀 Quick Setup

### 1. Create Workflow File

Create `.github/workflows/hugchat-bot.yml`:

```yaml
name: HugChat Issue Bot

on:
  issues:
    types: [opened, edited, closed, deleted]
  issue_comment:
    types: [created, edited]

jobs:
  hugchat-bot:
    runs-on: ubuntu-latest
    steps:
      - name: HugChat Issue Bot
        uses: lmangani/hugchat-issue-bot@v1.0.0
        with:
          hugchat-email: ${{ secrets.HUGCHAT_EMAIL }}
          hugchat-password: ${{ secrets.HUGCHAT_PASSWORD }}
          hugchat-assistant-id: ${{ secrets.HUGCHAT_ASSISTANTID }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

### 2. Add Repository Secrets

Go to your repository **Settings** → **Secrets and variables** → **Actions** and add:

| Secret Name | Description | Required |
|-------------|-------------|----------|
| `HUGCHAT_EMAIL` | Your HuggingFace email address | ✅ Yes |
| `HUGCHAT_PASSWORD` | Your HuggingFace password (not access token) | ✅ Yes |
| `HUGCHAT_ASSISTANTID` | Your custom assistant ID from HuggingFace | ❌ Optional |

> **Note**: Use your actual HuggingFace **password**, not an access token.

### 3. Get Your Assistant ID (Optional)

1. Go to [HuggingFace Chat Assistants](https://huggingface.co/chat/assistants)
2. Create or select your assistant
3. Copy the ID from the URL: `https://huggingface.co/chat/assistants/{ASSISTANTID}`
4. Add it as `HUGCHAT_ASSISTANTID` secret

## 📖 How It Works

### First Issue Comment
```
User creates issue: "How do I ... ?"
→ Bot creates new HugChat conversation
→ Bot responds with installation help
→ Response includes: "HugChat Session ID: `abc123`"
```

### Follow-up Comments
```
User comments: "What about ... ?"
→ Bot finds existing session ID `abc123`
→ Bot continues same conversation (with full context)
→ Bot provides Windows-specific help
→ Same session ID in response
```

### Issue Closed
```
Issue gets closed
→ Bot automatically deletes HugChat conversation `abc123`
→ Keeps your HugChat account clean
```

## ⚙️ Configuration

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `hugchat-email` | HuggingFace email address | Yes | - |
| `hugchat-password` | HuggingFace password | Yes | - |
| `hugchat-assistant-id` | Custom assistant ID | No | Default assistant |
| `github-token` | GitHub token for API access | Yes | `${{ secrets.GITHUB_TOKEN }}` |

### Advanced Usage

#### Custom Assistant
```yaml
- name: HugChat Issue Bot
  uses: lmangani/hugchat-issue-bot@v1.0.0
  with:
    hugchat-email: ${{ secrets.HUGCHAT_EMAIL }}
    hugchat-password: ${{ secrets.HUGCHAT_PASSWORD }}
    hugchat-assistant-id: ${{ secrets.HUGCHAT_ASSISTANTID }}
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

#### Multiple Repositories
The action works independently in each repository, maintaining separate conversation contexts.

## 🔧 Troubleshooting

### Common Issues

**❌ "wrong username or password"**
- Make sure you're using your actual HuggingFace password, not an access token
- Try logging into https://huggingface.co/chat manually to verify credentials
- Check if 2FA is enabled (may cause issues)

**❌ "No HugChat session found"**
- This is normal for new issues - the bot will create a new conversation
- For follow-ups, ensure previous bot responses include the session ID

**❌ "Failed to cleanup conversation"**
- Cleanup failures don't affect functionality
- The conversation may have already been deleted

### Enable Debug Logs

The action automatically shows debug information. Look for:
- `✓ Successfully maintaining conversation context!`
- `Conversation has X messages in history`
- `✓ Successfully cleaned up HugChat conversation`

## 🛡️ Security

- **Credentials**: Store HuggingFace credentials in GitHub Secrets
- **Privacy**: Conversation content is not logged in GitHub Actions
- **Cleanup**: Conversations are automatically deleted when issues close
- **Isolation**: Each repository maintains separate conversations

## 📋 Requirements

- HuggingFace account with HugChat access
- Repository with Issues enabled
- GitHub Actions enabled

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with a real HuggingFace account
5. Submit a pull request

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙋 Support

- Create an issue for bugs or feature requests
- Check the [HuggingFace Chat](https://huggingface.co/chat) documentation
- Review GitHub Actions logs for debugging information

---

**Made with ❤️ for the open source community**
