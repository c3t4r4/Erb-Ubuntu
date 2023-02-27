# Erb-Ubuntu
======

[![Build Status](https://app.travis-ci.com/srsran/srsRAN.svg?branch=master)](https://app.travis-ci.com/github/srsran/srsRAN)
[![Coverity](https://scan.coverity.com/projects/23045/badge.svg)](https://scan.coverity.com/projects/srsran)
======

## Passo a passo

0. Preparação:
    - Instale o Ubuntu 22.04 nos servidores diferentes.

    - Atualize as informações de pacote:
     ```sh
        sudo apt-add-repository universe && sudo apt update
     ```

    - Instalar Pacotes:
        ```sh
        sudo apt install bladerf git libbladerf-dev bladerf-fpga-hostedx40 libusb-1.0-0-dev libusb-1.0-0 build-essential cmake libncurses5-dev libtecla1 libtecla-dev pkg-config git wget libuhd-dev uhd-host libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev libboost-system-dev libboost-test-dev libboost-thread-dev libqwt-qt5-dev qtbase5-dev -y
        ```

1. Configurando UHD:

    - Download uhd images:
     ```sh
        sudo /usr/lib/uhd/utils/uhd_images_downloader.py
     ```

2. Instalar Programas:
    - SoapySDR
    ```sh
        cd ~/Documentos && git clone https://github.com/pothosware/SoapySDR.git && cd SoapySDR && git checkout tags/soapy-sdr-0.7.2 && mkdir build && cd build && cmake .. && make -j4 && sudo make install && sudo ldconfig
     ```

    - LimeSuite:
    ```sh
        cd ~/Documentos && git clone https://github.com/myriadrf/LimeSuite.git && cd LimeSuite && git checkout tags/v20.01.0 && mkdir builddir && cd builddir && cmake ../ && make -j4 && sudo make install && sudo ldconfig && cd .. && cd udev-rules && sudo ./install.sh
     ```

    - srsGUI:
    ```sh
        cd ~/Documentos && git clone https://github.com/srsLTE/srsGUI.git && cd srsGUI && mkdir build && cd build && cmake ../ && make && sudo make install
     ```

    - srsRAN:
        - Release Last:
        ```sh
            cd ~/Documentos && git clone https://github.com/srsRAN/srsRAN.git && cd srsRAN && mkdir build && cd build && cmake ../ && make -j4 && sudo make install && sudo ldconfig && sudo ./srsran_install_configs.sh user
        ```

        - Release 19_12:
        ```sh
            cd ~/Documentos && git clone https://github.com/srsRAN/srsRAN.git && cd srsRAN && git checkout tags/release_19_12 && mkdir build && cd build && cmake ../ && make -j4 && sudo make install && sudo ldconfig && sudo ./srslte_install_configs.sh user
        ```

    - srsRANFork:
    ```sh
        cd ~/Documentos && git clone https://github.com/c3t4r4/srsRAN-SN.git && cd srsRAN-SN && mkdir build && cd build && cmake ../ && make -j4 && sudo make install && sudo ldconfig && sudo ./srsran_install_configs.sh user
     ```

3. Configurar srsRan:
    - Configurar ENB:
    ```sh
        sudo nano /root/.config/srsran/enb.conf
    ```

    ```txt
        [enb]
        mcc = <yourMCC>
        mnc = <yourMNC>
        mme_addr = 127.0.1.100     ## or IP for external MME, eg. 192.168.1.10
        gtp_bind_addr = 127.0.1.1  ## or local interface IP for external S1-U, eg. 192.168.1.3
        s1c_bind_addr = 127.0.1.1  ## or local interface IP for external S1-MME, eg. 192.168.1.3
        n_prb = 15
        tm = 2
        nof_ports = 2

        [rf]
        dl_earfcn = 1934
        tx_gain = 80               ## this power seems to work best
        rx_gain = 40
        device_name = bladeRF
        device_args = auto         ## does not work with anything other than 'auto'
    ```
    
    - Configurar EPC:
    ```sh
        sudo nano /root/.config/srsran/epc.conf
    ```

    ```txt
        [mme]
        mcc = <yourMCC>
        mnc = <yourMNC>
        mme_bind_addr = 127.0.1.100  ## or local interface IP for external S1-MME, eg. 192.168.1.10
    ```

4. Usando Amplificador XB-300:
    - Ativando
    ```sh
        bladeRF-cli -i

    ```
    ```sh
        xb 300 enable
        
    ```
    ```sh
        xb 300 pa on
        
    ```
    ```sh
        xb 300 lna on
        
    ```

    