# Scoop Bucket

[![Tests](https://github.com/haomingz/scoop-bucket/actions/workflows/ci.yml/badge.svg)](https://github.com/haomingz/scoop-bucket/actions/workflows/ci.yml) [![Excavator](https://github.com/haomingz/scoop-bucket/actions/workflows/excavator.yml/badge.svg)](https://github.com/haomingz/scoop-bucket/actions/workflows/excavator.yml)

Personal [Scoop](https://scoop.sh) bucket for useful apps not found in the official buckets.

## Usage

```powershell
scoop bucket add haomingz https://github.com/haomingz/scoop-bucket
scoop install haomingz/<app-name>
```

## Apps

| App | Description | Homepage |
|-----|-------------|----------|
| [alma](./bucket/alma.json) | AI-powered assistant and productivity tool | [alma.now](https://alma.now) |
| [claude-code](./bucket/claude-code.json) | An agentic coding tool that lives in your terminal | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| [configarr](./bucket/configarr.json) | Configuration tool for arr services (Radarr, Sonarr, etc.) | [configarr.de](https://configarr.de/) |
| [icann-rdap](./bucket/icann-rdap.json) | ICANN implementation of the Registry Data Access Protocol (RDAP) | [github.com/icann/icann-rdap](https://github.com/icann/icann-rdap) |
| [mcp-grafana](./bucket/mcp-grafana.json) | A Model Context Protocol (MCP) server for Grafana | [github.com/grafana/mcp-grafana](https://github.com/grafana/mcp-grafana) |

## Adding New Apps

1. **Create** a new JSON file in the `bucket` directory:

```powershell
cp bucket/app-name.json.template bucket/your-app-name.json
```

2. **Fill in** the manifest:

```json
{
    "version": "1.0.0",
    "description": "Short description",
    "homepage": "https://example.com",
    "license": "MIT",
    "architecture": {
        "64bit": {
            "url": "https://example.com/app-v1.0.0-win-x64.zip",
            "hash": "sha256:..."
        }
    },
    "bin": "app.exe",
    "checkver": {
        "github": "https://github.com/username/repo"
    },
    "autoupdate": {
        "architecture": {
            "64bit": {
                "url": "https://example.com/app-v$version-win-x64.zip"
            }
        }
    }
}
```

3. **Get the hash**:

```powershell
scoop hash path/to/downloaded/file.zip
```

4. **Test** the manifest:

```powershell
scoop install bucket/your-app-name.json
```

5. **Commit** with message: `feat(manifest): add app-name v1.0.0`

## References

- [Scoop Wiki](https://github.com/ScoopInstaller/Scoop/wiki)
- [App Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)
- [Creating an App Manifest](https://github.com/ScoopInstaller/Scoop/wiki/Creating-an-app-manifest)

## License

MIT License
