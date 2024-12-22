# Guide: How to make Rails 8 run on a M1 MacBook 

[Rails 8](https://edgeguides.rubyonrails.org/8_0_release_notes.html) finally came out and instead of simply upgrading and getting started with all of its new and cool features, I faced tons of red error messages, telling me my MacBook is not willing to run Ruby or install some of the gems needed. What a bummer! But after some failed attempts using `rvm` and `rbenv`, luckily I found a way to make it work!

To successfully install Ruby and Rails 8 on your M1 MacBook running macOS Sequoia 15.1, you'll likely encounter compatibility issues with ARM64 and the installation process can be quite bumpy. Here’s how I managed to install Rails 8 and all necessary dependencies, using `asdf`:

## 0. Activate Rosetta 2

Feel free to try going through the whole process without Rosetta first.
In my case, I was only able to successfully install everything after activating it.
Here you can find Apple's Guide on 
[How to install and activate Rosetta on your Mac](https://support.apple.com/en-us/102527).

## 1. Prerequisites

Install Xcode Command Line Tools
```bash
xcode-select --install  
```

Install Homebrew (if not already installed)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  
```

Follow the post-installation instructions to add Homebrew to your path (something like `echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile`).

Update Homebrew and Install Dependencies
```bash
brew update  
brew install openssl readline libyaml zlib gpg autoconf automake bison libffi jq  
```

## 2. Install asdf
Clone and set up asdf
```bash
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.13.1  
echo '. $HOME/.asdf/asdf.sh' >> ~/.zshrc  
echo '. $HOME/.asdf/completions/asdf.```bash' >> ~/.zshrc  
source ~/.zshrc  
```

Install asdf Plugins
```bash
asdf plugin add ruby  
asdf plugin add nodejs  
```

For Node.js, asdf requires GPG to verify downloads:
```bash
bash ~/.asdf/plugins/nodejs/bin/import-release-team-keyring  
```

## 3. Install Ruby
Set up Ruby Dependencies

For ARM64, ensure correct paths:

```bash
export RUBY_CONFIGURE_OPTS="--with-opt-dir=$(brew --prefix openssl) --enable-shared"  
```

Install Ruby via asdf
```bash
asdf install ruby latest  
asdf global ruby latest  
```

Verify the installation:
```bash
ruby --version
#=> ruby 3.3.5 (2024-09-03 revision ef084cc8f4) [x86_64-darwin24] 
```

You can also check the installed plugins. The output should look something like this: 
```bash
asdf list 
#=> nodejs
#=>  *22.4.0
#=> ruby
#=>  *3.3.5
```

## 4. Install Rails 8
Update RubyGems

```bash
gem update --system  
```

Install Rails

```bash
gem install rails  
```

Again, confirm that Rails is installed and works:
```bash
rails --version
#=> Rails 8.0.0
```

## 5. Additional Configuration

Configure Bundler

```bash
gem install bundler  
```

Create a New Rails Project

```bash
rails new myapp  
cd myapp  
bin/rails server 
```

Visit `http://localhost:3000` to confirm everything is running correctly.

### CONGRATULATIONS! 
NOW YOU CAN HAVE FUN DIVING INTO RAILS 8 💎❤️🛤️



