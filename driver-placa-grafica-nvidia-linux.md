Driver placa gráfica NVidia Linux

O meu XPS-15 tem uma placa gráfica da NVidia, ie:

    joao@joao-XPS-15-9500:~$ lspci | grep -i nvidi
    01:00.0 3D controller: NVIDIA Corporation TU117M [GeForce GTX 1650 Ti Mobile] (rev a1)


E parece-me que o driver que estou a utilizar é um genérico nouveau, em vez do específico driver da NVidia.

    joao@joao-XPS-15-9500:~$ lsmod | egrep "^video "
    video                  73728  4 dell_wmi,dell_laptop,i915,nouveau

Para informação mais completa:

    joao@joao-XPS-15-9500:~$ sudo lshw -c video
      *-display
           description: 3D controller
           product: TU117M [GeForce GTX 1650 Ti Mobile]
           vendor: NVIDIA Corporation
           physical id: 0
           bus info: pci@0000:01:00.0
           version: a1
           width: 64 bits
           clock: 33MHz
           capabilities: pm msi pciexpress bus_master cap_list rom
           configuration: driver=nouveau latency=0
           resources: irq:180 memory:b3000000-b3ffffff memory:70000000-7fffffff memory:80000000-81ffffff ioport:3000(size=128)
      *-display
           description: VGA compatible controller
           product: CometLake-H GT2 [UHD Graphics]
           vendor: Intel Corporation
           physical id: 2
           bus info: pci@0000:00:02.0
           logical name: /dev/fb0
           version: 05
           width: 64 bits
           clock: 33MHz
           capabilities: pciexpress msi pm vga_controller bus_master cap_list rom fb
           configuration: depth=32 driver=i915 latency=0 mode=3840x2400 resolution=3840,2400 visual=truecolor xres=3840 yres=2400
           resources: iomemory:600-5ff iomemory:400-3ff irq:179 memory:604a000000-604affffff memory:4000000000-400fffffff ioport:4000(size=64) memory:c0000-dffff

Portanto, para a placa gráfica normal, o driver i915 parece ser apropriado, mas para o controlador 3D, `driver=nouveau` não parece ser o ideal.

Humnn, valerá a pena fazer a alteração ?  Penso que sim.  Vamos tentar.  Uma pesquisa rápida aponta para <https://askubuntu.com/questions/481414/install-nvidia-driver-instead-of-nouveau>.  Esse artigo é antigo, a resposta com mais votos é sobre Ubuntu 14, e eu estou a usar Ubuntu 22.

Olhando para as outras respostas, parece que basta abriar a aplicação `Software & Updates`, escolher a tab `Additional Drivers` e escolher a opção NVIDIA em vez de Nouveau, esperar que a atualização termine e reiniciar o computador.  Básico e simples.

E depois de re-iniciar:

    joao@joao-XPS-15-9500:~$ sudo lshw -c video
      *-display
           description: 3D controller
           product: TU117M [GeForce GTX 1650 Ti Mobile]
           vendor: NVIDIA Corporation
           physical id: 0
           bus info: pci@0000:01:00.0
           version: a1
           width: 64 bits
           clock: 33MHz
           capabilities: pm msi pciexpress bus_master cap_list rom
           configuration: driver=nvidia latency=0
           resources: irq:180 memory:b3000000-b3ffffff memory:70000000-7fffffff memory:80000000-81ffffff ioport:3000(size=128)
      *-display
           description: VGA compatible controller
           product: CometLake-H GT2 [UHD Graphics]
           vendor: Intel Corporation
           physical id: 2
           bus info: pci@0000:00:02.0
           logical name: /dev/fb0
           version: 05
           width: 64 bits
           clock: 33MHz
           capabilities: pciexpress msi pm vga_controller bus_master cap_list rom fb
           configuration: depth=32 driver=i915 latency=0 mode=3840x2400 resolution=3840,2400 visual=truecolor xres=3840 yres=2400
           resources: iomemory:600-5ff iomemory:400-3ff irq:179 memory:604a000000-604affffff memory:4000000000-400fffffff ioport:4000(size=64) memory:c0000-dffff


`driver=nvidia` aparece no driver do controlador 3D, tudo parece funcionar.

Tags: linux, nvidia, nouveau
