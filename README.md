# Google Cloud GPU Instance
Instructions for setting up a GPU instance on Google Cloud
This setup is for:
	- tensorflow-gpu==1.7
	- CUDA9.0
	- cudnn v7
1. Request limit increase for instances  
2. Put in number of GPUs & CPUs, Size:  
https://console.cloud.google.com/compute/quotas?hl=de&_ga=1.69181649.1124953923.1496188820  

3. Create GPU instance:
![GPU1](images/GPU_1.png)  

4.  
![GPU2](images/GPU_2.png)  

5. GPU are only available in certain areas:  
![GPU_Area](images/GPU_area.png)  

6.  
- Choose Boot disk -> Ubuntu 16.04 LTS  
- Choose 30 GB HDD  
![GPU3](images/GPU_3.png)  

7. Select zone, number of GPUs & CPUs and memory  
![GPU4](images/GPU_4.png)  

8. Allow HTTP & HTTPS traffic  
![GPU6](images/GPU_6.png)  

9. Instance is set up  
![GPU7](images/GPU_7.png)  

10. Connect to instance:    

		ssh username@remote  

![GPU8](images/GPU_8.png)  

11. Install GPU drivers: 

		wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
		sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
		sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
		sudo apt-get update  
		sudo apt-get install cuda-9-0  

16. Insert lines in bashrc:  
 
		echo 'export CUDA_HOME=/usr/local/cuda' >> ~/.bashrc  
		echo 'export PATH=$PATH:$CUDA_HOME/bin' >> ~/.bashrc  
		echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_HOME/lib64' >> ~/.bashrc  
		source ~/.bashrc
 

17. Download cuDNN from Nvidia Developer site -> Sign Up required  
- https://developer.nvidia.com/rdp/cudnn-download  
- Save on laptop: 
https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.0.5/prod/9.0_20171129/cudnn-9.0-linux-x64-v7

18. Open terminal on laptop and copy file to instance:  
 
		scp cudnn-9.0-linux-x64-v7.tgz *INSTANCE_NAME*:~


19. Go to terminal on instance and extract & install:  
 
		tar -xzvf cudnn-9.0-linux-x64-v7.tgz  
		sudo cp cuda/lib64/* /usr/local/cuda/lib64/  
		sudo cp cuda/include/cudnn.h /usr/local/cuda/include/  
		rm -rf ~/cuda  
		rm cudnn-9.0-linux-x64-v7.tgz  
		sudo apt-get update   

20. Install python packages:  

		sudo apt-get install python-pip  
		sudo apt-get install python-numpy python-scipy python-matplotlib ipython ipython-notebook  
		sudo apt-get install python-pandas python-sympy 

21. Install Anaconda:  
 
		wget https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh  
		bash Anaconda3-4.4.0-Linux-x86_64.sh   

22. Set Anaconda as preferred environment:  
- `which python` should be `usr/bin/python`  

- Important:  

		source .bashrc  
23. Create and activate conda environment

		conda create -n env python=2.7
		source activate env
- Generate config file:  

		jupyter notebook --generate-config
		
- Configure jupyter password:  

		jupyter notebook password  
		
23. Run this on the remote server:  
		
		jupyter notebook --no-browser --port=8887

24. Run this on your local computer

		ssh -N -L localhost:8888:localhost:8887 username@remote
		
		
25. Install Keras and Tensorflow:

		pip/conda install keras
		pip/conda install tensorflow-gpu
		conda install opencv
		
26. If GPU process is not running, try to run a keras/tf model first:  

		nvidia-smi  
		
![GPU10](images/GPU_10.png) 


### Sources / Appendix

https://medium.com/google-cloud/running-jupyter-notebooks-on-gpu-on-google-cloud-d44f57d22dbd  
https://cloud.google.com/compute/docs/gpus/add-gpus?hl=de#install-driver-manual  
https://cloud.google.com/compute/docs/gcloud-compute/?hl=de  

- One line code to create GPU instance:

		gcloud beta compute instances create gpu-inst1 --machine-type n1-standard-2 --zone us-west1-b --accelerator type=nvidia-tesla-k80, count=1 --image-family ubuntu-1604-lts --image-project ubuntu-os-cloud --boot-disk-size 30GB --maintenance-policy TERMINATE --restart-on-failure




		

		
		



