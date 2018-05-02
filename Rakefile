require 'html-proofer'

ASCIIDOCTOR_DECJS = './tmp/asciidoctor-deck.js'
TEST_BUILD = './test_build'
DECKJS_LINK = File.join(TEST_BUILD, 'deck.js')
IMAGES_DIR = File.expand_path('../images', __FILE__)
CUSTOM_CSS = File.expand_path('../css/custom.css', __FILE__)

def cook_test_env
  FileUtils.mkdir_p TEST_BUILD
  FileUtils.rm_rf(File.join(TEST_BUILD, '/*'));
  FileUtils.ln_s '../deck.js', TEST_BUILD  unless File.exist?(DECKJS_LINK)
end

COMMON_ATTRIBUTES = {
      'imagesdir' => IMAGES_DIR,
      'imagesoutdir' => IMAGES_DIR,
      'lang' => 'ru',
      'encoding' => 'utf-8',
      'author' => 'Leonid Vlasov',
      'deckjs_theme' => 'swiss',
      'navigation' => nil,
      'goto' => nil,
      'menu' => nil,
      'status' => nil,
      'split' => nil,
      'source-highlighter' => 'coderay',
      'coderay-linenums-mode' => 'inline',
      'customcss' => CUSTOM_CSS,
      'safe-mode-safe' => nil,
      'data-uri' => nil,
      'cache-uri' => nil}

def common_attributes
  attr_to_cmd COMMON_ATTRIBUTES
end

def attr_to_cmd(attr)
  r = []
  attr.each do |a, v|
    r << " -a #{a}#{v.nil? ? '' : "=#{v.gsub(' ', '\\ ')}"}"
  end
  r.join(' ')
end

def asciidoctor_deckjs
  fail "Deckjs beckend not exist. Clone it:\n"\
    '  $mkdir -p tmp && git clone'\
    ' https://github.com/asciidoctor/asciidoctor-deck.js.git'\
    " #{ASCIIDOCTOR_DECJS}" unless File.exist?(ASCIIDOCTOR_DECJS)
  File.join(ASCIIDOCTOR_DECJS, 'templates/haml')
end

def asciidoctor(src, dest_dir, attributes)
  "asciidoctor -r asciidoctor-diagram -T #{asciidoctor_deckjs} #{attributes} #{src} -D #{dest_dir}"
end

def build(src, dest_dir, attributes = common_attributes)
  fail ArgumentError, "Path not exist `#{src}'" unless File.exist?(src)
  fail ArgumentError, "Path not exist `#{dest_dir}'" unless File.exist?(dest_dir)
  `#{asciidoctor(src, dest_dir, attributes)}`
  File.join(dest_dir, "#{File.basename(src, 'asciidoc')}.html")
end

def all_asciidoc
  Dir.glob('./src/*.{asciidoc,adoc}')
end

def lint(file)
  HTMLProofer.check_file(file).run
  file
end

desc 'Build project'
task :build do |t|
  all_asciidoc.each do |f|
    lint build(f, './')
  end
end

desc 'Test build for [file.asciidoc]'
task :test, :src do |t, args|
  cook_test_env
  lint build(args[:src], TEST_BUILD)
end

desc 'Test build for all *.asciidoc files'
task :test_all do
  cook_test_env
  all_asciidoc.each do |file|
    lint build(file, TEST_BUILD)
  end
end

task default: :test_all
