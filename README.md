# NS8 Postiz Module

[![NethServer 8](https://img.shields.io/badge/NethServer-8-blue)](https://github.com/geniusdynamics/ns8-core)
[![License](https://img.shields.io/badge/License-GPL--3.0-green)](LICENSE)

A [NethServer 8](https://github.com/geniusdynamics/ns8-core) module that integrates **Postiz** - a powerful open-source social media scheduling tool.

## What is Postiz?

[Postiz](https://postiz.com) is a self-hosted platform that allows you to schedule and manage posts across multiple social media platforms from a single dashboard. It provides:

- **Multi-platform scheduling** for Twitter/X, LinkedIn, Facebook, Instagram, Threads, TikTok, YouTube, Reddit, Discord, and more
- **Content calendar** with visual scheduling
- **Team collaboration** with role-based access
- **Media library** for managing images and videos
- **Analytics and insights** for post performance
- **AI-powered features** for content generation
- **Workflow automation** with Temporal

This module brings Postiz's social media management capabilities to your NethServer infrastructure.

## Features

- 🚀 Easy scheduling across 15+ social platforms
- 🔒 Integrated with NethServer's security model
- 🌐 Automatic domain configuration with Traefik
- 📊 Web-based management interface
- 🔄 Automated workflows with Temporal
- 🗄️ PostgreSQL and Redis for data management
- 📱 Mobile-friendly UI
- 🤖 AI integration ready (OpenAI)

## Installation

### Prerequisites

- NethServer 8 cluster
- At least 2GB RAM available
- Docker/Podman support

### Install the Module

Instantiate the module with:

```bash
add-module ghcr.io/geniusdynamics/postiz:latest 1
```

The output will return the instance name:

```json
{
  "module_id": "postiz1",
  "image_name": "postiz",
  "image_url": "ghcr.io/geniusdynamics/postiz:latest"
}
```

## Configuration

Launch `configure-module` with the following parameters:

- `host`: Fully qualified domain name for Postiz
- `temporal_host`: Fully qualified domain name for Temporal (workflow engine)
- `http2https`: Enable/disable HTTP to HTTPS redirection (`true`/`false`)
- `lets_encrypt`: Enable/disable Let's Encrypt certificate (`true`/`false`)

### Example Configuration

```bash
api-cli run configure-module --agent module/postiz1 --data - <<EOF
{
  "host": "postiz.yourdomain.com",
  "temporal_host": "temporal.yourdomain.com",
  "http2https": true,
  "lets_encrypt": true
}
EOF
```

This will:

- Start and configure the Postiz instance
- Start and configure the Temporal workflow engine
- Set up a virtual host in Traefik for external access
- Configure automatic HTTPS if enabled

### Advanced Configuration

You can also configure additional parameters through the web UI or API:

- **Social Platform Integrations**: GitHub, YouTube, Dribbble, Facebook, X/Twitter, Threads, TikTok, Slack, LinkedIn, Mastodon, Pinterest, Reddit, Discord
- **Storage Settings**: Local or cloud storage providers
- **AI Features**: OpenAI API integration
- **Stripe Integration**: For payment processing
- **Registration Settings**: Enable/disable user registration
- **API Limits**: Rate limiting configuration

## Usage

1. **Access Postiz**: Navigate to `https://postiz.yourdomain.com`
2. **Initial Setup**: Register your admin account
3. **Connect Social Accounts**: Authorize your social media platforms
4. **Schedule Posts**: Use the content calendar to plan and schedule posts
5. **Monitor Analytics**: Track post performance and engagement

## Module Management

### Get Configuration

```bash
api-cli run get-configuration --agent module/postiz1
```

### Update Module

To update the Postiz module to the latest version:

```bash
api-cli run update-module --data '{"module_url":"ghcr.io/geniusdynamics/postiz:latest","instances":["postiz1"],"force":true}'
```

This will:

- Pull the latest container images
- Restart services with the new version
- Preserve all data and configurations

### Uninstall

```bash
remove-module --no-preserve postiz1
```

## Advanced Configuration

### Smarthost Integration

The module automatically discovers and integrates with NethServer's centralized [smarthost setup](https://nethserver.github.io/ns8-core/core/smarthost/). When the smarthost configuration changes, the module automatically reloads its services.

### Services

The module consists of multiple services:

- **postiz-app**: Main Postiz application
- **postiz-postgres**: PostgreSQL database
- **postiz-redis**: Redis cache
- **temporal**: Workflow engine for background jobs
- **temporal-ui**: Temporal web interface
- **temporal-postgresql**: Temporal database
- **temporal-elasticsearch**: Temporal search index
- **spotlight**: Search and analytics

### Environment Variables

The module runs with comprehensive environment variables managed by the NethServer agent. Key variables include database connection details, social media API keys, and security settings.

## Development & Debugging

### Debug Commands

Check environment variables:

```bash
runagent -m postiz1 env
```

Enter the module's runtime environment:

```bash
runagent -m postiz1
```

Inspect running containers:

```bash
runagent -m postiz1
podman ps
```

Access container environment:

```bash
podman exec postiz-app env
```

Open shell in container:

```bash
podman exec -ti postiz-app sh
```

### View Logs

```bash
runagent -m postiz1
journalctl -u postiz-app -f
```

### UI Development

The module includes a Vue.js-based web interface. For UI development:

1. Navigate to the `ui/` directory
2. Follow the [NethServer UI development guide](https://nethserver.github.io/ns8-core/ui/modules/#module-ui-development)

## Testing

Test the module using the provided script:

```bash
./test-module.sh <NODE_ADDR> ghcr.io/geniusdynamics/postiz:latest
```

Tests are written using [Robot Framework](https://robotframework.org/).

## Translation

The UI supports multiple languages and is translated via [Weblate](https://hosted.weblate.org/projects/ns8/).

To contribute translations:

- Add the [GitHub Weblate app](https://docs.weblate.org/en/latest/admin/continuous.html#github-setup) to your repository
- Add your repository to [hosted.weblate.org](https://hosted.weblate.org) or request addition to the NS8 Weblate project

## Support & Contributing

- 📖 [NethServer Documentation](https://nethserver.github.io/ns8-core/)
- 🐛 [Report Issues](https://github.com/geniusdynamics/ns8-postiz/issues)
- 💬 [Community Forum](https://community.nethserver.org/)
- 🤝 [Contributing Guide](CONTRIBUTING.md)

## License

This project is licensed under the GPL-3.0 License - see the [LICENSE](LICENSE) file for details.
