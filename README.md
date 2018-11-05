### Photish
---
https://github.com/henrylawson/photish

```
brew install imagemagick
brew install exiftool

sudo apt-get install imagemagick
sudo apt-get install libimage-exiftool-perl

sudo yum install ImageMagic
sudo yum install perl-Image-ExifTool

gem install photish

mkdir my_new_photo_site
cd my_new_photo_site
bundle init
echo 'gem "photish"' >> Gemfile
bundle install

echo "deb https://dl.bintray.com/henrylawson/deb all main" | sudo tee -a /etc/apt/sources.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys XXXXXXXXX

sudo apt-get update && sudo apt-key update
sudo apt-get install photish

sudo dpkg -i photish*.deb

wget https://bintray.com/henrylawson/rpm/rpm -O bintray-henrylawson-rpm.repo
sudo mv bintray-henrylawson-rpm.repo /etc/yum.repos.d/
sudo yum install -y photish

tar -zxf photish-*-linux-*.tar.gz -C /destination

photish init --example
photish init

photish g --config_override='{"logging":{"colorize":false}}'

photish generate

photish g --force
photish host --force
photish host
photish deploy --engine=name

git clone git@github.com:henrylawson/photish.git
cd photish
./bin/setup
rake
vim
./bin/console

```

```
port: 9876
qualities:
  - name: Original
    params: []
  - name: Low
    params: ['-resize', '200x200']
templates:
  layout: layout.slim
  collection: collection.slim
  album: album.slim
  photo: photo.slim
logging:
  colorize: true
  level: 'debug'
  output: ['stdout', 'file']
url:
  host: http://mydomain.com
  base: 'subdirectory'
  type: 'relative_uri'
workers: 4
threads: 2
force: false
plugins: ['ssh_deploy', 'other_plugin']
image_extensions: ['jpg', 'gif']
page_extension: 'slim'
dependencies:
  minimagick:
    cli: 'imagemagick'
    cli_path: '/usr/bin/convert'
    timeout: 3600
  miniexiftool:
    command: '/usr/bin/'

// site/_templates/layout.{type}
doctype html
html
  head
    title Master Layout
  body
    == yeild
    
// site/_templates/collection.{type}
h1 Photo Collection
- albums.each do |album|
  div.album
    div.album-title
      a href=album.url
        | #{album.name}

// site/_templates/album.{type}
h1 Album
h2
  a href=url
    | #{name}
div.album-photos
  - photos.each do |photo|
    div.album-photo
      a href=photo.url title=photo.name
        img src=photo.images.find{ |i|i.quality_name=='Low' }.url alt=photo.name
        
// site/_templates/photo.{type}
h1 Photo
h2
  a href=url
    | #{name}
  div.album-photos
    - images.each do |image|
      div.album-photo
        a href=image.url title=image.name
          img src=image.url alt=image.quality_name
          
div.content
  div.site-breadcrumbs
    == breadcrumbs
    
<div class="content">
</div>

div.more-about-album
  h2 The More Details Page
  p Lorem ipsum...
  p Lorem ipsum...
  p Lorem ipsum...
  
// site/index/all_albums.html.{page_extension}
doctype html
html
  head
    title All Albums
  body
    ol
      - all_albums.each do |album|
        li #{album.name}
        
// site/_templates/photo.slim
div.my-shouting-content
  == shout("HELLO")

dependencies:
  minimagick:
    cli: graphicsmagick
    
dependencies:
  minimatick:
    cli_paht: '/usr/bin/convert'
```

```ruby
gem 'photish-plugin-sshdeploy'

plugins: ['photish/plugin/sshdeploy']

# site/_plugins/my_custom_deploy.rb
module Photish::Plugin::MyCustomDeploy
  def initialize(config, log)
    @config = config
    @log = log
  end
  def self.is_for?(type)
    Photish::Plugin::Type::Photo == type
  end
  def self.engine_name
    'my_custom_deploy'
  end
  def deploy_site
    @log.debug "Deploying using my plugin"
    FileUtils.cp(@config.output_dir, '/src/www')
  end
end



Photish::Rake::Task.new(:generate, 'Compiles the project to HTML') do |t|
  t.options = "generate"
end

# site/_plugins/hout.rb
module Photish::Plugin::Shout
  def self.is_for?(type)
    Photish::Plugin::Type::Photo == type
  end
  def shout(message)
    "<strong>I am shouting '#{message}'!!!</strong>"
  end
end

```
