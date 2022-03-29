# UbuntuのインストールとCUDA,cuDNNの入れ方
- ## CUDAのバージョン確認
```
lspci | grep -i nvidia
```
    
![GPU-version](https://user-images.githubusercontent.com/50350039/160253215-512a8396-8678-4abd-818a-4fdd62eb99f8.png)
  
- ## nvcc --versionとnvidia-smiのCUDAバージョンが違う
  
    ## 以下のURLの文章を参考する

    https://stackoverflow.com/questions/53422407/different-cuda-versions-shown-by-nvcc-and-nvidia-smi

- ## CUDA対応しているバージョン
    | CUDA Toolkit               | Linux x86_64 Driver Version |
    | -------------------------- | --------------------------- |
    | CUDA 9.2(9.2.88)           | >=396.26                    |
    | CUDA 9.2(9.2.148 Update 1) | >=396.37 && <410.48         |
    | CUDA 10.1                  | >=418.39                    |
    | CUDA 10.2.89               | >=440.33                    |
    | CUDA 11.0.1 RC             | >=450.36.06                 |
    | CUDA 11.0.2 GA             | >=450.51.05                 |
    | CUDA 11.0.3 Update 1       | >=450.51.06                 |

- ## 既存のNvidiaドライバを削除
  
    ### 以下のコマンドをターミナルで実行して、既存で入っているCUDAやnvidia-driverなどを削除します。
    
```
sudo apt --purge remove nvidia-*
sudo apt --purge remove cuda-*
```
- ## ``今回はUbuntu20.4でCUDA11.1、cuDNN 8.0.5をインストール``
　このサイトでCUDA11.1をダウンロード
  https://developer.nvidia.com/cuda-11.1.1-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=2004&target_type=runfilelocal

  ## <font color=Red>以下のコマンドターミナルで打ち込む</font>

  ```
  wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda_11.1.1_455.32.00_linux.run

  sudo chmod +x cuda_11.1.1_455.32.00_linux.run 

  sudo sh cuda_11.1.1_455.32.00_linux.run
  ```
![cuda](https://user-images.githubusercontent.com/50350039/160553072-67da8712-cf39-47c3-85a9-fad48557a061.png)

```
gedit ~/.bashrc 
```
## bashrcの最後尾で

```
export PATH=$PATH:/usr/local/cuda-11.1/bin

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.1/lib64

export CUDADIR=/usr/local/cuda-11.1
```
```
source ~/.bashrc //シェルの設定再起動しても「.bashrc」に記載した内容を即座に反映させる

nvcc -V //cudaのバージョンを確認するためのコマンド
```
![nvcc](https://user-images.githubusercontent.com/50350039/160557153-ce1637ca-a464-4def-8824-27e97f070705.png)

- ## ``cuDNNをインストール``
  ## CUDA11.1に対応したcuDNNのバージョンは8.0.5

  過去にリリースしたcuDNNのバージョンをダウンロード　https://developer.nvidia.com/rdp/cudnn-archive

```
tar -xzvf cudnn-11.1-linux-x64-v8.0.5.39.tgz 

sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 

sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 

sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*


sudo apt install nvidia-driver-460
```

