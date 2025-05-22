# GitHub CLI

`gh` is GitHub on the command line. It brings pull requests, issues, and other GitHub concepts to the terminal next to where you are already working with `git` and your code.

![screenshot of gh pr status](https://user-images.githubusercontent.com/98482/84171218-327e7a80-aa40-11ea-8cd1-5177fc2d0e72.png)

GitHub CLI is supported for users on GitHub.com, GitHub Enterprise Cloud, and GitHub Enterprise Server 2.20+ with support for macOS, Windows, and Linux.

## Documentation

For [installation options see below](#installation), for usage instructions [see the manual][manual].

## Contributing

If anything feels off, or if you feel that some functionality is missing, please check out the [contributing page][contributing]. There you will find instructions for sharing your feedback, building the tool locally, and submitting pull requests to the project.

If you are a hubber and are interested in shipping new commands for the CLI, check out our [doc on internal contributions][intake-doc].

<!-- this anchor is linked to from elsewhere, so avoid renaming it -->
## Installation

### macOS

`gh` is available via [Homebrew][], [MacPorts][], [Conda][], [Spack][], [Webi][], and as a downloadable binary including Mac OS installer `.pkg` from the [releases page][].

> [!NOTE]
> As of May 29th, Mac OS installer `.pkg` are unsigned with efforts prioritized in [`cli/cli#9139`](https://github.com/cli/cli/issues/9139) to support signing them.

#### Homebrew

| Install:          | Upgrade:          |
| ----------------- | ----------------- |
| `brew install gh` | `brew upgrade gh` |

#### MacPorts

| Install:               | Upgrade:                                       |
| ---------------------- | ---------------------------------------------- |
| `sudo port install gh` | `sudo port selfupdate && sudo port upgrade gh` |

#### Conda

| Install:                                 | Upgrade:                                |
|------------------------------------------|-----------------------------------------|
| `conda install gh --channel conda-forge` | `conda update gh --channel conda-forge` |

Additional Conda installation options available on the [gh-feedstock page](https://github.com/conda-forge/gh-feedstock#installing-gh).

#### Spack

| Install:           | Upgrade:                                 |
| ------------------ | ---------------------------------------- |
| `spack install gh` | `spack uninstall gh && spack install gh` |

#### Webi

| Install:                            | Upgrade:         |
| ----------------------------------- | ---------------- |
| `curl -sS https://webi.sh/gh \| sh` | `webi gh@stable` |

For more information about the Webi installer see [its homepage](https://webinstall.dev/).

#### Flox

| Install:          | Upgrade:                |
| ----------------- | ----------------------- |
| `flox install gh` | `flox upgrade toplevel` |

For more information about Flox, see [its homepage](https://flox.dev)

### Linux & BSD

`gh` is available via:
- [our Debian and RPM repositories](./docs/install_linux.md);
- community-maintained repositories in various Linux distros;
- OS-agnostic package managers such as [Homebrew](#homebrew), [Conda](#conda), [Spack](#spack), [Webi](#webi); and
- our [releases page][] as precompiled binaries.

For more information, see [Linux & BSD installation](./docs/install_linux.md).

### Windows

`gh` is available via [WinGet][], [scoop][], [Chocolatey][], [Conda](#conda), [Webi](#webi), and as downloadable MSI.

#### WinGet

| Install:            | Upgrade:            |
| ------------------- | --------------------|
| `winget install --id GitHub.cli` | `winget upgrade --id GitHub.cli` |

> [!NOTE]
> The Windows installer modifies your PATH. When using Windows Terminal, you will need to **open a new window** for the changes to take effect. (Simply opening a new tab will _not_ be sufficient.)

#### scoop

| Install:           | Upgrade:           |
| ------------------ | ------------------ |
| `scoop install gh` | `scoop update gh`  |

#### Chocolatey

| Install:           | Upgrade:           |
| ------------------ | ------------------ |
| `choco install gh` | `choco upgrade gh` |

#### Signed MSI

MSI installers are available for download on the [releases page][].

### Codespaces

To add GitHub CLI to your codespace, add the following to your [devcontainer file](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/adding-features-to-a-devcontainer-file):

```json
"features": {
  "ghcr.io/devcontainers/features/github-cli:1": {}
}
```

### GitHub Actions

GitHub CLI comes pre-installed in all [GitHub-Hosted Runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners).

### Other platforms

Download packaged binaries from the [releases page][].

#### Verification of binaries

Since version 2.50.0 `gh` has been producing [Build Provenance Attestation](https://github.blog/changelog/2024-06-25-artifact-attestations-is-generally-available/) enabling a cryptographically verifiable paper-trail back to the origin GitHub repository, git revision and build instructions used. The build provenance attestations are signed and relies on Public Good [Sigstore](https://www.sigstore.dev/) for PKI.

There are two common ways to verify a downloaded release, depending if `gh` is already installed or not. If `gh` is installed, it's trivial to verify a new release:

- **Option 1: Using `gh` if already installed:**

  ```shell
  $ gh at verify -R cli/cli gh_2.62.0_macOS_arm64.zip
  Loaded digest sha256:fdb77f31b8a6dd23c3fd858758d692a45f7fc76383e37d475bdcae038df92afc for file://gh_2.62.0_macOS_arm64.zip
  Loaded 1 attestation from GitHub API
  ✓ Verification succeeded!

  sha256:fdb77f31b8a6dd23c3fd858758d692a45f7fc76383e37d475bdcae038df92afc was attested by:
  REPO     PREDICATE_TYPE                  WORKFLOW
  cli/cli  https://slsa.dev/provenance/v1  .github/workflows/deployment.yml@refs/heads/trunk
  ```

- **Option 2: Using Sigstore [`cosign`](https://github.com/sigstore/cosign):**

  To perform this, download the [attestation](https://github.com/cli/cli/attestations) for the downloaded release and use cosign to verify the authenticity of the downloaded release:

  ```shell
  $ cosign verify-blob-attestation --bundle cli-cli-attestation-3120304.sigstore.json \
        --new-bundle-format \
        --certificate-oidc-issuer="https://token.actions.githubusercontent.com" \
        --certificate-identity="https://github.com/cli/cli/.github/workflows/deployment.yml@refs/heads/trunk" \
        gh_2.62.0_macOS_arm64.zip
  Verified OK
  ```

### Build from source

See here on how to [build GitHub CLI from source][build from source].

## Comparison with hub

For many years, [hub][] was the unofficial GitHub CLI tool. `gh` is a new project that helps us explore
what an official GitHub CLI tool can look like with a fundamentally different design. While both
tools bring GitHub to the terminal, `hub` behaves as a proxy to `git`, and `gh` is a standalone
tool. Check out our [more detailed explanation][gh-vs-hub] to learn more.

[manual]: https://cli.github.com/manual/
[Homebrew]: https://brew.sh
[MacPorts]: https://www.macports.org
[winget]: https://github.com/microsoft/winget-cli
[scoop]: https://scoop.sh
[Chocolatey]: https://chocolatey.org
[Conda]: https://docs.conda.io/en/latest/
[Spack]: https://spack.io
[Webi]: https://webinstall.dev
[releases page]: https://github.com/cli/cli/releases/latest
[hub]: https://github.com/github/hub
[contributing]: ./.github/CONTRIBUTING.md
[gh-vs-hub]: ./docs/gh-vs-hub.md
[build from source]: ./docs/source.md
[intake-doc]: ./docs/working-with-us.md

## Remote CLI Access via SSH

### Installing Docker

1. **Install Docker**:
   - Ensure Docker is installed on your PC. You can download it from Docker's official website.

### Cloning the Repository

1. **Clone the Repository**:
   - Clone the repository `docfhsp/cli` to your local machine.

### Setting Up the Development Container

1. **Set Up the Dev Container**:
   - Navigate to the repository directory and set up the development container using the `.devcontainer/devcontainer.json` file. This file specifies the Docker image and features required for the development environment.
   - Run the following command to build and start the development container:
     ```sh
     docker-compose up -d
     ```

### Configuring SSH

1. **Configure SSH**:
   - The `.devcontainer/devcontainer.json` file includes the SSHD feature, which sets up an SSH server in the container.
   - Ensure you have SSH installed on your mobile phone. You can use apps like Termux (for Android) or any SSH client available for iOS.
   - Connect to the SSH server running in the Docker container using the following command:
     ```sh
     ssh vscode@<your-pc-ip-address> -p <ssh-port>
     ```
   - Replace `<your-pc-ip-address>` with the IP address of your PC and `<ssh-port>` with the port number configured for SSH in the container.

### Generating SSH Keys on the Mobile Phone

1. **Install an SSH Client**:
   - On Android, you can use Termux or JuiceSSH.
   - On iOS, you can use apps like Blink Shell or Termius.

2. **Generate SSH Keys**:
   - Open the SSH client app and run the following command to generate a new SSH key pair:
     ```sh
     ssh-keygen -t rsa -b 2048
     ```
   - Follow the prompts to save the key pair. By default, it will be saved in `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`.

### Copying the Public Key to the PC

1. **Copy the Public Key**:
   - Display the public key using the following command:
     ```sh
     cat ~/.ssh/id_rsa.pub
     ```
   - Copy the displayed key to your clipboard.

2. **Add the Public Key to the PC**:
   - On the PC, open the `~/.ssh/authorized_keys` file (create it if it doesn't exist) and paste the copied public key into the file.
   - Ensure the file has the correct permissions:
     ```sh
     chmod 600 ~/.ssh/authorized_keys
     ```

### Connecting to the PC's CLI from the Mobile Phone

1. **Connect Using the SSH Client**:
   - Open the SSH client app on your mobile phone.
   - Use the following command to connect to your PC:
     ```sh
     ssh username@your_pc_ip_address
     ```
   - Replace `username` with your PC's username and `your_pc_ip_address` with the IP address of your PC.

2. **Verify the Connection**:
   - If prompted, accept the server's fingerprint.
   - You should now have access to your PC's CLI from your mobile phone.

### Using Termux on Android to Access SSH

1. **Install Termux**:
   - Download and install Termux from the Google Play Store or F-Droid.

2. **Install OpenSSH in Termux**:
   - Open Termux and run the following command to install OpenSSH:
     ```sh
     pkg install openssh
     ```

3. **Generate SSH Keys in Termux**:
   - Run the following command to generate a new SSH key pair:
     ```sh
     ssh-keygen -t rsa -b 2048
     ```
   - Follow the prompts to save the key pair. By default, it will be saved in `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`.

4. **Copy the Public Key to the PC**:
   - Display the public key using the following command:
     ```sh
     cat ~/.ssh/id_rsa.pub
     ```
   - Copy the displayed key to your clipboard.
   - On the PC, open the `~/.ssh/authorized_keys` file (create it if it doesn't exist) and paste the copied public key into the file.
   - Ensure the file has the correct permissions:
     ```sh
     chmod 600 ~/.ssh/authorized_keys
     ```

5. **Connect to the PC's CLI from Termux**:
   - Use the following command to connect to your PC:
     ```sh
     ssh username@your_pc_ip_address
     ```
   - Replace `username` with your PC's username and `your_pc_ip_address` with the IP address of your PC.
   - If prompted, accept the server's fingerprint.
   - You should now have access to your PC's CLI from Termux on your Android mobile phone.
