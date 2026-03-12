# pri-campos.github.io
Personal knowledge base and technical articles

## Pré-requisitos

Este site utiliza **Jekyll + GitHub Pages (Ruby)**.

Instale:

### macOS
```bash
brew install rbenv ruby-build
```

Ative no shell (zsh):
```bash
echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
source ~/.zshrc
```

Instale a versão correta do Ruby:
```bash
rbenv install 3.2.2
rbenv global 3.2.2
```

Instale bundler:
```bash
gem install bundler
```

### Start (desenvolvimento local)

Primeira vez:
```bash
bundle install
```

Rodar local:
```bash
bundle exec jekyll serve --livereload
```

Abrir:
http://127.0.0.1:4000
