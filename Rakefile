require './constants'

task :clean do
  sh <<-EOS
rm -f docs/*.pbf
  EOS
end

def fill(src, z, x, y) 
  src.sub('{z}', "#{z}").sub('{x}', "#{x}").sub('{y}', "#{y}")
end

task :build do
  html = `curl #{INDEX_URL}`
  html.sub!(SOURCE_TEMPLATE_URL, STAGE_TEMPLATE_URL)
  File.open(INDEX_PATH, 'w') {|w|
    w.print html
  }
  Z.downto(MINZOOM) {|z|
    dz = Z - z
    x = X >> dz
    y = Y >> dz
    src_url = fill(SOURCE_TEMPLATE_URL, z, x, y)
    dst_path = fill(STAGE_PATH, z, x, y)
    print "#{src_url} -> #{dst_path}\n"
    File.open(dst_path, 'w') {|w|
      w.print `curl #{src_url}`
    }
  }
end

