---
title: "CentOS AutoSDのカーネル(っぽいもの)のコンフィグを見てみた"
type:
topics: ["Linux","CentOS Stream"]
published: true
---

# CentOS AutoSD?

CentOSプロジェクトから、CentOS Automotive Stream Distribution(AutoSD)が発表されました。

https://blog.centos.org/2022/03/centos-automotive-sig-announces-new-autosd-distro

![autosdflow](https://blog.centos.org/wp-content/uploads/2022/03/autosd.png)
※画像はCentOS Projectの発表ページより

何を目指しているLinux Distributionなのか、などのコンセプチャルなところは発表記事を見ればわかるのですが、実態としてAutoSDは何が普通のCentOS Streamと違うのでしょう。今のところはまったく説明がないです。ですが、[CentOS Automotive SIG](https://wiki.centos.org/SpecialInterestGroup/Automotive)の活動の成果と思しきものがGitLabにてオープンになっています。

https://gitlab.com/redhat/automotive

どうやら、`kernel-automotive`というパッケージの存在が見てとれます。Linux Kernelですね。CentOS Stream 9と同じベースバージョン(5.14)を採用しているようです。
どんなオプションが有効になっているのか気になったので、CentOS Stream 9のカーネルコンフィグと比べてみることにしました。
比較したのは記事作成当時(2022/03/05)の[CentOS Stream 9の最新のkernel](https://gitlab.com/redhat/centos-stream/rpms/kernel/-/blob/833c32d1aca1abc22bdd495ed009c4e6f72097f3/kernel-aarch64-rhel.config)と[CentOS Automotive SIGの最新のkernel-automotive](https://gitlab.com/redhat/automotive/rpms/kernel-automotive/-/blob/5abc7d9ab38dc508ffb4d804210271d017075fa1/SOURCES/kernel-automotive-aarch64-rhel.config)です。まずはaarch64が面白そうかなと思い見てみました。

diffは[ここ](https://github.com/kenya888/zenn-articles/blob/main/articles/kernel-automotive-config.diff)に置いてありますので、他に気になる違いがあるかどうか見てみてください。

## Raspberry Piっぽい?

BroadcomのBCM2835など、Raspberry PiのSoCの機能を有効にするコンフィグが散見されます。

```diff
+CONFIG_ARCH_BCM2835=y
+CONFIG_BCM2835_MBOX=m
+CONFIG_BCM2835_POWER=y
+CONFIG_BCM2835_THERMAL=m
+CONFIG_BCM2835_TIMER=y
+CONFIG_BCM2835_VCHIQ=m
+CONFIG_BCM2835_VCHIQ_MMAL=m
+CONFIG_BCM2835_WDT=m
+CONFIG_CLK_BCM2835=y
+CONFIG_DMA_BCM2835=m
+CONFIG_HW_RANDOM_BCM2835=y
+CONFIG_I2C_BCM2835=m
+CONFIG_MMC_BCM2835=m
+CONFIG_PINCTRL_BCM2835=y
+CONFIG_PWM_BCM2835=m
+CONFIG_SERIAL_8250_BCM2835AUX=m
+CONFIG_SND_BCM2835=m
+CONFIG_SND_BCM2835_SOC_I2S=m
+CONFIG_SPI_BCM2835AUX=m
+CONFIG_SPI_BCM2835=m
+CONFIG_VIDEO_BCM2835=m
+CONFIG_ARM_RASPBERRYPI_CPUFREQ=m
+CONFIG_CLK_RASPBERRYPI=m
+CONFIG_GPIO_RASPBERRYPI_EXP=m
+CONFIG_PWM_RASPBERRYPI_POE=m
+CONFIG_RASPBERRYPI_FIRMWARE=m
+CONFIG_RESET_RASPBERRYPI=m
+CONFIG_SENSORS_RASPBERRYPI_HWMON=m
```

車載に実際にRaspberry Piを採用することはないと思うので、おそらく開発ボードとして想定しているのでしょう。

## Qualcomm っぽい?

おそらく実際の車載機器に採用される可能性があるQualcommのSoCが持っていそうな機能に関するコンフィグです。どうなんでしょうね。

```diff
+CONFIG_ARM_QCOM_CPUFREQ_HW=m
+CONFIG_ARM_QCOM_CPUFREQ_NVMEM=m
+CONFIG_CRYPTO_DEV_QCOM_RNG=m
+CONFIG_EDAC_QCOM=m
+CONFIG_HWSPINLOCK_QCOM=m
+CONFIG_I2C_QCOM_CCI=m
+CONFIG_I2C_QCOM_GENI=m
+CONFIG_MMC_QCOM_DML=y
+CONFIG_PCIE_QCOM=y
+CONFIG_PHY_QCOM_PCIE2=m
+CONFIG_PHY_QCOM_QMP=m
+CONFIG_PHY_QCOM_QUSB2=m
+CONFIG_PHY_QCOM_USB_HS_28NM=m
+CONFIG_PHY_QCOM_USB_HSIC=m
+CONFIG_PHY_QCOM_USB_HS=m
+CONFIG_PHY_QCOM_USB_SNPS_FEMTO_V2=m
+CONFIG_PHY_QCOM_USB_SS=m
+CONFIG_PINCTRL_QCOM_SPMI_PMIC=m
+CONFIG_QCOM_AOSS_QMP=m
+CONFIG_QCOM_APCS_IPC=m
+CONFIG_QCOM_BAM_DMA=m
+CONFIG_QCOM_COMMAND_DB=m
+CONFIG_QCOM_CPR=m
+CONFIG_QCOM_GENI_SE=m
+CONFIG_QCOM_GPI_DMA=m
+CONFIG_QCOM_GSBI=m
+CONFIG_QCOM_IOMMU=y
+CONFIG_QCOM_IPA=m
+CONFIG_QCOM_IPCC=y
+CONFIG_QCOM_LLCC=m
+CONFIG_QCOM_LMH=m
+CONFIG_QCOM_PDC=m
+CONFIG_QCOM_Q6V5_ADSP=m
+CONFIG_QCOM_Q6V5_MSS=m
+CONFIG_QCOM_Q6V5_PAS=m
+CONFIG_QCOM_Q6V5_WCSS=m
+CONFIG_QCOM_QFPROM=m
+CONFIG_QCOM_RMTFS_MEM=m
+CONFIG_QCOM_RPMH=m
+CONFIG_QCOM_RPMHPD=m
+CONFIG_QCOM_SCM=y
+CONFIG_QCOM_SMEM=m
+CONFIG_QCOM_SMP2P=m
+CONFIG_QCOM_SMSM=m
+CONFIG_QCOM_SOCINFO=m
+CONFIG_QCOM_SYSMON=m
+CONFIG_QCOM_TSENS=m
+CONFIG_QCOM_WCNSS_PIL=m
+CONFIG_RPMSG_QCOM_GLINK_RPM=m
+CONFIG_RPMSG_QCOM_GLINK_SMEM=m
+CONFIG_RPMSG_QCOM_SMD=m
+CONFIG_SCSI_UFS_QCOM=m
+CONFIG_SERIAL_QCOM_GENI_CONSOLE=y
+CONFIG_SERIAL_QCOM_GENI=m
+CONFIG_SPI_QCOM_GENI=m
```

## NVIDIAっぽい？

NVIDIAのSoCのTEGRAシリーズの機能を有効にするオプションが追加されています。
MAX77620はMaximのPMICですが、[ここ](https://ic.maximintegrated.com/MAX77620-request)とか見るとJetsonシリーズと一緒に使われそうなことが推測できますね。
仮にJetsonシリーズが開発ボードとして使えると色々と捗るのではないでしょうか。

```diff
+CONFIG_ARCH_TEGRA_194_SOC=y
+CONFIG_ARM_TEGRA186_CPUFREQ=m
+CONFIG_ARM_TEGRA194_CPUFREQ=y
+CONFIG_GPIO_TEGRA186=y
+CONFIG_I2C_TEGRA_BPMP=m
+CONFIG_PCIE_TEGRA194_HOST=m
+CONFIG_PHY_TEGRA194_P2U=m
+CONFIG_SERIAL_TEGRA_TCU=m
+CONFIG_TEGRA_BPMP_THERMAL=m
+CONFIG_TEGRA_BPMP=y
+CONFIG_TEGRA_HSP_MBOX=y
+CONFIG_TEGRA_IVC=y
+CONFIG_TEGRA_MC=y

+CONFIG_GPIO_MAX77620=m
+CONFIG_MAX77620_THERMAL=m
+CONFIG_MAX77620_WATCHDOG=m
+CONFIG_MFD_MAX77620=y
+CONFIG_PINCTRL_MAX77620=m
+CONFIG_REGULATOR_MAX77620=m
```

## その他

USB-OTGドライバっぽいものとか

```diff
+CONFIG_USB_DWC3_HAPS=m
+CONFIG_USB_DWC3_HOST=y
+CONFIG_USB_DWC3=m
+CONFIG_USB_DWC3_OF_SIMPLE=m
+CONFIG_USB_DWC3_PCI=m
+CONFIG_USB_DWC3_QCOM=m
```

まぁこれから色々追加削除変更される可能性もありますし、将来これらが本当に使えるかを約束するものではないです。

いかがでしょう。CentOSプロジェクトのような、パッケージングとソフトウェアディストリビューション技術のバックグラウドがしっかりしてそうなプロジェクトから車載用Linuxが出る可能性が出てきたのはけっこう面白いことなんじゃないかと思います。またAutomotive SIGの成果として、他の車載に必要なユーザーランドのソフトウェアパッケージも登場するかもしれません。今後注目していこうと思います。興味のある方はAutomotive SIGのメーリングリストに参加してみるのもよいと思います(私も参加してみました)

https://lists.centos.org/mailman/listinfo/centos-automotive-sig

ではでは。

