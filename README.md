

# GDANCE: "Music-Driven Group Choreography"
## Abstract
> Music-driven choreography is a challenging problem with a wide variety of industrial applications. Recently, many methods have been proposed to synthesize dance motions from music for a single dancer. However, generating dance motion for a group remains an open problem. In this paper, we present GDANCE, a new large-scale dataset for music-driven group dance generation. Unlike existing datasets that only support single dance, our new dataset contains group dance videos, hence supporting the study of group choreography. We propose a semi-autonomous labeling method with humans in the loop to obtain the 3D ground truth for our dataset. The proposed dataset consists of 16.7 hours of paired music and 3D motion from in-the-wild videos, covering 7 dance styles and 16 music genres. We show that naively applying single dance generation technique to creating group dance motion may lead to unsatisfactory results, such as inconsistent movements and collisions between dancers. Based on our new dataset, we propose a new method that takes an input music sequence and a set of 3D positions of dancers to efficiently produce multiple group-coherent choreographies. We propose new evaluation metrics for measuring group dance quality and perform intensive experiments to demonstrate the effectiveness of our method. Our code and dataset will be released to facilitate future research on group dance generation.

## Dataset Structure
**[Download]** The dataset can be downloaded at [Link](https://vision.aioz.io/f/cc712c8bc57e41d5a6ad/?dl=1)

The data directory is organized as follows:
- **split_sequence_names.txt**:
    -   a txt file containing seperate sequence names in the data (each sequence should have unique name or id)
- **musics**:
    -  contains raw music .wav file of each sequence with the corresponding name. The music frames are aligned with the motion frames.
- **motions_smpl**:
    -  contains the motion file of each sequence with the corresponding name, the motion is provided in .pkl file format.
    -  Each data dictionary mainly includes the following items:
        - `'smpl_poses': shape[num_persons x num_frames x 72]`: the motions contain 72-D vector pose sequences in SMPL pose format (24 joints).
        - ``'root_trans': shape[num_persons x num_frames x 3]``: sequences of root translation.

Here is an example python script to read the motion file
```python
import pickle
import numpy as np
data = pickle.load(open("sequence_name.pkl","rb"))
print(data.keys())

smpl_poses = data['smpl_poses']
smpl_trans = data['root_trans']

# ... may utilize the pose by using SMPL forward function: https://github.com/vchoutas/smplx
```

## Visualizing
We provide demo code for loading and visualizing the motions. 

### Prerequisites
First, you need to download the [SMPL model](https://smpl.is.tue.mpg.de/) (v1.0.0) and rename the model files for visualization. The directory structure of the data is expected to be:

The directory structure of the data is expected to be:
```
<DATA_DIR>
├── motions_smpl/
├── musics/
└── split_sequence_names.txt

<SMPL_DIR>
├── SMPL_MALE.pkl
└── SMPL_FEMALE.pkl
```

Then run this to install the necessary packages
```
pip install scipy torch smplx chumpy vedo trimesh
pip install numpy==1.23.0
```

### Usage
#### Visualize the SMPL joints
The following command will first calculate the SMPL joint locations (joint rotations and root translation) and then plot on the 3D figure in realtime.
``` bash
python vis_smpl_kpt.py \
  --data_dir <ANNOTATIONS_DIR>/motions_smpl \
  --smpl_path <SMPL_DIR>/SMPL_FEMALE.PKL \
  --sequence_name sequence_name.pkl
```

#### Visualize the SMPL Mesh
The following command will calculate the SMPL meshes and visualize in 3D. 
``` bash
python vis_smpl_mesh.py \
  --data_dir <ANNOTATIONS_DIR>/motions_smpl \
  --smpl_path <SMPL_DIR>/SMPL_FEMALE.PKL \
  --sequence_name sequence_name.pkl
```

## Citation
```
@article{aiozGdance,
    author    = {Le, Nhat and Pham, Thang and Do, Tuong and Tjiputra, Erman and Tran, Quang D. and Nguyen, Anh},
    title     = {Music-Driven Group Choreography},
    journal   = {CVPR},
    year      = {2023},
}		
```

## License
This data is available for non-commercial scientific research purposes as defined in the LICENSE file. By downloading and using this data you agree to the terms in the LICENSE. Third-party datasets and software are subject to their respective licenses.



