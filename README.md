  
base is ChronoMonochrome/lineage-15.1-old  
  
1.firmware replaced  
  
2.64bit binder enabled   
  
3.usb adb driver is newered  
  
4.for camera work, delete ARCH_USES_LEGACY_MMAP  
modified arch/arm/mach-exynos/Kconfig  
``` sh  
config CPU_EXYNOS4412 
        bool  
        select S3C_PL330_DMA  
        select ARM_ERRATA_761320  
        select ARCH_USES_LEGACY_MMAP   <--- deleted.  
        help  
        Enable EXYNOS4412 CPU support  
```   
modified arch/arm/mm/mmap.c (removed ARCH_USES_LEGACY_MMAP) 
  
#### 2020/12/25  
 max clock set to 1.4GHz  
 modify arch/arm/mach-exynos/cpufreq.c  
 modify arch/arm/mach-exynos/cpufreq-4x12c  

#### for lineage, defconfig's FB_MDNIE_RGB_ADJUST is need not define.
CONFIG_FB_MDNIE_RGB_ADJUST is not set  
  


