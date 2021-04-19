
Base is  https://github.com/ChronoMonochrome/android_kernel_samsung_smdk4412/tree/lineage-16.0-i9100-rebase

Clock up for 9.0 kernel referenced by CustomRoms-lineage-16.0
```
cp $HOME/temp/CustomRoms-lineage-16.0/include/linux/sysfs_helpers.h $HOME/crdroid-9.0/kernel/samsung/smdk4412dcm/include/linux/
```
arch/arm/mach-exynos/Kconfig
```
        select ARM_ERRATA_751472
        select ARM_ERRATA_754322
        select ARM_ERRATA_764369
+       select BUSFREQ
        help
          Samsung EXYNOS4 series based systems
```
arch/arm/mach-exynos/cpufreq-4x12.c
```
redefined CONFIG_MACH_T0 -> CONFIG_MACH_M3
```
arch/arm/mach-exynos/cpufreq.c
```
redefined CONFIG_MACH_T0 -> CONFIG_MACH_M3
```
arch/arm/mach-exynos/include/mach/cpufreq.h
```
redefined CONFIG_MACH_T0 -> CONFIG_MACH_M3
```
include/linux/cpufreq.h
```c

 #define CPUFREQ_NAME_LEN 16

+/* CPU UV DEFINES */
+#define CPU_UV_MV_MAX 1500000
+#define CPU_UV_MV_MIN 600000

 /*********************************************************************
  *                     CPUFREQ NOTIFIER INTERFACE                    *
@@ -35,6 +37,7 @@
 #ifdef CONFIG_CPU_FREQ
 int cpufreq_register_notifier(struct notifier_block *nb, unsigned int list);
 int cpufreq_unregister_notifier(struct notifier_block *nb, unsigned int list);
+extern void disable_cpufreq(void);
 #else          /* CONFIG_CPU_FREQ */
 static inline int cpufreq_register_notifier(struct notifier_block *nb,
                                                unsigned int list)
@@ -46,6 +49,7 @@
 {
        return 0;
 }
+static inline void disable_cpufreq(void) { }
 #endif         /* CONFIG_CPU_FREQ */



 #define cpufreq_freq_attr_rw(_name)            \
 static struct freq_attr _name =                \
-__ATTR(_name, 0644, show_##_name, store_##_name)
+__ATTR(_name, 0666, show_##_name, store_##_name)


 #define define_one_global_rw(_name)            \
 static struct global_attr _name =              \
-__ATTR(_name, 0644, show_##_name, store_##_name)
+__ATTR(_name, 0666, show_##_name, store_##_name)


 #ifdef CONFIG_CPU_FREQ
 unsigned int cpufreq_quick_get(unsigned int cpu);
+unsigned int cpufreq_quick_get_max(unsigned int cpu);
 #else
 static inline unsigned int cpufreq_quick_get(unsigned int cpu)
 {
        return 0;
 }
+static inline unsigned int cpufreq_quick_get_max(unsigned int cpu)
+{
+       return 0;
+}
 #endif


 void cpufreq_frequency_table_put_attr(unsigned int cpu);

+#define SCALING_MAX_COUPLED 1
+#define SCALING_MAX_UNDEFINED 0
+#define SCALING_MAX_UNCOUPLED -1

 #endif /* _LINUX_CPUFREQ_H */
```

arch/arm/config/lineageos_sc03e_defconfig
```
#
# Busfreq Model
#
CONFIG_BUSFREQ=y
# CONFIG_BUSFREQ_QOS is not set
CONFIG_BUSFREQ_OPP=y
# CONFIG_DISPFREQ_OPP is not set
# CONFIG_DEVFREQ_BUS is not set
CONFIG_BUSFREQ_QOS_NONE=y
# CONFIG_BUSFREQ_QOS_1024X600 is not set
# CONFIG_BUSFREQ_QOS_1280X720 is not set
# CONFIG_BUSFREQ_QOS_1280X800 is not set
# CONFIG_BUSFREQ_DEBUG is not set
# CONFIG_BUSFREQ_L2_160M is not set
```

Deleted boeffla_sound

sound/soc/codecs/Makefile
```
-snd-soc-wm8994-objs := wm8994.o wm8994-tables.o wm8958-dsp2.o boeffla_sound.o
+snd-soc-wm8994-objs := wm8994.o wm8994-tables.o wm8958-dsp2.o
```
sound/soc/codecs/wm8994.c
```
 #include "wm8994.h"
 #include "wm_hubs.h"

-#include "boeffla_sound.h"
-
-
 #define WM1811_JACKDET_MODE_NONE  0x0000
 #define WM1811_JACKDET_MODE_JACK  0x0100
 #define WM1811_JACKDET_MODE_MIC   0x0080
@@ -197,8 +194,6 @@

        BUG_ON(reg > WM8994_MAX_REGISTER);

-       value = Boeffla_sound_hook_wm8994_write(reg, value);
-
        if (!wm8994_volatile(codec, reg)) {
                ret = snd_soc_cache_write(codec, reg, value);
                if (ret != 0)

                break;
        }

-       Boeffla_sound_hook_wm8994_pcm_probe(codec);
-
        return 0;
```


