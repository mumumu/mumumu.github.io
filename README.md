## mumumu's public blog

### How to setup

1. install Ruby

```
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
$ . ~/.bashrc
$ mkdir -p "$(rbenv root)"/plugins
$ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
$ rbenv install -l
$ rbenv install [your-favorite-ruby-version]
$ rbenv global [your-favorite-ruby-version]
```

2. clone blog source

```
$ git clone git@github.com:mumumu/mumumu.github.io.git blog
$ cd blog
$ git checkout -b source origin/source
```

3. restore setup

```
$ gem install bundler
$ bundle install
$ mkdir _deploy
$ cd _deploy
$ git init
$ git remote add origin git@github.com:mumumu/mumumu.github.io
```


