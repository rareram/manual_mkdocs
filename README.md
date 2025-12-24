# 라이브러리 설치

uv init
uv add mkdocs mkdocs-material mkdocs-pdf-export-plugin

# Theme

uv add mkdocs-rtd-dropdown mkdocs-material mkdocs-bootstrap

theme:
  name: 'rtd-dropdown'
  name: 'material'
  name: 'bootstrap'

# 실행

uv run mkdocs --version
uv run mkdocs new .
uv run mkdocs serve
