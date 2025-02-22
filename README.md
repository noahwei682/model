# model

cd ~
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh -b -p ~/miniconda3
echo 'export PATH="/home/aiscuser/miniconda3/bin:$PATH"' >> ~/.bashrc            <!-- 替换本地conda环境安装路径 -->
source ~/.bashrc

apt update
apt install sudo
sudo apt update
sudo apt install wget
wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda_12.1.0_530.30.02_linux.run
sudo sh cuda_12.1.0_530.30.02_linux.run --silent --toolkit
echo 'export PATH=/usr/local/cuda-12.1/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.1/lib64:$$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
nvcc -V



cp /blob/weiwei/envs/llava_packed.tar.gz /home/aiscuser/miniconda3/envs/       <!-- 替换hf下载的llava_packed.tar.gz路径，替换本地conda环境路径 -->
cd /home/aiscuser/miniconda3/envs/
mkdir llava
tar -xzvf llava_packed.tar.gz -C /home/aiscuser/miniconda3/envs/llava         <!-- 替换本地conda llava环境路径 -->
source activate llava; conda activate base ; conda activate llava
pip install ipdb
pip install --upgrade huggingface-hub
conda install -c conda-forge wandb --yes
wandb login a0686d210ceba8f713f6cd85c5dcf3621b7f15e7
source activate /home/aiscuser/miniconda3/envs/llava                          <!-- 替换本地conda llava环境路径 -->
conda activate base
conda activate /home/aiscuser/miniconda3/envs/llava                            <!-- 替换本地conda llava环境路径 -->


cd ~ 
cd mycode
cp -r /blob/weiwei/codes/LLaVA_EWC /home/aiscuser/mycode/                    <!-- 替换github下载的模型代码路径 替换放置代码 路径-->
cd LLaVA_EWC/
pip install --upgrade pip 
pip install -e .
pip install -e ".[train]"
pip install flash-attn --no-build-isolation
git pull
pip install -e .
pip install ipdb
conda install -c conda-forge ftfy

pip install --upgrade huggingface-hub
conda install -c conda-forge wandb --yes
wandb login a0686d210ceba8f713f6cd85c5dcf3621b7f15e7                      
export CKPT_DIR=EVA02-CLIP-L-14-336-LLaVA_EWC                    
export PRETRAIN_JSON=blip_laion_cc_sbu_558k.json
export EVA_CKPT=/blob/weiwei/logs/EVA-CLIP/cross_attention/T_vit_1024x4_lr1e-5_Rcc12mR_Rcc3m_Rdatacamp-2024_09_16-21/checkpoints/epoch_2/mp_rank_00_model_states.pt     <!-- 替换hf下载的视觉模型路径-->
bash ./scripts/v1_5/eval/check_ckpt_dit.sh
export OPENAI_KEY=06e3e62fff2245c0b2e982f33a65cb11 
PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True bash run_all.sh
