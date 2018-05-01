require 'asciidoctor-diagram'

ASCIIDOCTOR_DECJS = './tmp/asciidoctor-deck.js'

TEST_BUILD = './test_build'
FileUtils.mkdir_p TEST_BUILD

COMMON_ATTRIBUTES = {
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
      'customcss' => 'css/custom.css',
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
  "asciidoctor -T #{asciidoctor_deckjs} #{attributes} #{src} -D #{dest_dir}"
end

def build(src, dest_dir, attributes = common_attributes)
  fail ArgumentError, "Path not exist `#{src}'" unless File.exist?(src)
  fail ArgumentError, "Path not exist `#{dest_dir}'" unless File.exist?(dest_dir)
  `#{asciidoctor(src, dest_dir, attributes)}`
  File.join(dest_dir, "#{File.basename(src, 'asciidoc')}.html")
end

def all_asciidoc
  Dir.glob('./src/*.asciidoc')
end

def lint(file)
  #TODO: lint links
  file
end

desc 'Build project'
task :build do |t|
  fail 'FIXME'
end

desc 'Test build for [file.asciidoc]'
task :test, :src do |t, args|
  lint build(args[:src], TEST_BUILD)
end

desc 'Test build for all *.asciidoc files'
task :test_all do
  all_asciidoc.each do |file|
    lint build(file, TEST_BUILD)
  end
end

task default: :test_all
