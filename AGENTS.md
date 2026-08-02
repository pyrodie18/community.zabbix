# community.zabbix repository guidance

- This repository must be located at `/workspace/ansible_collections/community/zabbix` in the development container.
- `origin` is Troy Ward's fork and `upstream` is the official `ansible-collections/community.zabbix` repository. Never push to `upstream` unless the user explicitly requests it.
- Bootstrap Python dependencies with `uv sync --frozen --group dev` and verify the lock with `uv lock --check`.
- Install exact development collection dependencies with `uv run --frozen ansible-galaxy collection install -r requirements.yml -p /home/vscode/.ansible/collections`.
- Run Python and changelog checks with `uv run --frozen flake8 plugins` and `uv run --frozen antsibull-changelog lint`.
- Audit Ansible lint with `uv run --frozen ansible-lint`. Modern ansible-lint currently reports pre-existing debt across Molecule and integration fixtures, so this is not a blocking migration check.
- Run the repository's established sanity scope with `uv run --frozen ansible-test sanity --exclude molecule/ --python 3.12`.
- Run unit tests, when `tests/unit` exists, with `uv run --frozen ansible-test units --python 3.12`.
- Build release artifacts with `uv run --frozen ansible-galaxy collection build --force --output-path dist`.
- Test an artifact with `uv run --frozen ansible-galaxy collection install --force --no-deps dist/community-zabbix-*.tar.gz -p /tmp/collection-install`.
- Run Molecule and Docker-backed integration scenarios only from their explicit GitHub workflows. The normal devcontainer has no Docker daemon or host socket.
- Add controller/test packages with `uv add --dev`; do not install them directly with `pip`.
- Keep exact development collection versions in `requirements.yml`; keep publish-time compatibility ranges in `galaxy.yml`.
- Do not read or modify sibling repositories or files outside this repository unless the user explicitly requests it.
- Do not add host Docker access, secrets, caches, generated test output, or Codex state to this repository.
