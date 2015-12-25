class Build < Thor
  desc "ova", "Build ova."
  method_option :output, :type => :string, :desc => "Output filename."
  def ova(source='https://downloads.openwrt.org/chaos_calmer/15.05/x86/64/openwrt-15.05-x86-64-combined-ext4.img.gz')
    output = options["output"] || "/tmp/openwrt.ova"
    puts "Downloading OpenWRT image..."
    system("curl -#SL \"#{source}\" | gzip -cd > /tmp/openwrt.img")
    puts "Convert image to vmdk..."
    system("qemu-img convert -O vmdk /tmp/openwrt.img /tmp/openwrt.vmdk")
    IO.write("/tmp/openwrt.vmx", %{
displayName = "OpenWRT"
config.version = "8"
virtualHW.version = "11"
memsize = "256"
mem.hotadd = "TRUE"
sata0.present = "TRUE"
sata0:0.present = "TRUE"
sata0:0.fileName = "openwrt.vmdk"
ethernet0.present = "TRUE"
ethernet0.connectionType = "nat"
ethernet0.virtualDev = "e1000"
ethernet0.wakeOnPcktRcv = "FALSE"
ethernet0.addressType = "generated"
guestOS = "other3xlinux-64"
})
    puts "Building ova..."
    exec("ovftool", "/tmp/openwrt.vmx", output)
    puts "Build successful."
  end
end
