# Gensyn_AI
this is my personalized for best gensyn output 

# Use this in rented GPU where we are in Container and dont have sudo command access

# Step 1: System Update and Core Dependencies
First, update your package list and install the essential tools and libraries required for compiling software and managing your system.

# Update package lists and upgrade existing packages
```
apt update && apt upgrade -y
```
# Install essential build tools, utilities, and libraries
```
apt install curl build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop pkg-config libssl-dev libleveldb-dev tar clang ncdu unzip -y

```

# Step 2: Install Python Environment
Next, install Python 3 and the necessary tools for managing Python packages (pip) and creating virtual environments (venv).

# Install Python 3, pip, venv, and development headers
```
apt install python3 python3-pip python3-venv python3-dev -y
```

# Step 3: Install Node.js and Yarn
The application requires Node.js and Yarn. These commands will add the Node.js repository, install it, and then install Yarn.


# Add the NodeSource repository for Node.js v22
```
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
```
# Install Node.js (which includes npm)
```
apt install -y nodejs
```
# Install Yarn using the official script
```
curl -o- -L https://yarnpkg.com/install.sh | bash
```
# IMPORTANT: Add Yarn to your PATH for the current session.
# You should also add this line to your ~/.bashrc file to make it permanent.
```
export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"
```
```
source ~/.bashrc
```
# Verify installations (optional)
```
node -v
yarn -v
```

# Step 4: Clone the Gensyn rl-swarm Repository
With all the dependencies installed, you can now download the project code from GitHub.

# Clone the repository into a new folder named 'rl-swarm'
```
git clone https://github.com/gensyn-ai/rl-swarm/
```

# Step 5: Configure and Run the Node
Finally, navigate into the project directory, set up the Python environment, ensure the code is up-to-date, and run the main script. It's recommended to run this inside a tmux session so it continues running if you disconnect.

# Attach to an existing tmux session
```
tmux
```

# Navigate into the project directory
```
cd rl-swarm
```

# Create and activate a local Python virtual environment
```
python3 -m venv .venv
source .venv/bin/activate
```

# Ensure your local code is clean and up-to-date with the main branch
```
# Clean and update repository (consolidated)
git switch main && git reset --hard && git clean -fd && git pull origin main
```

# IF NEED TO EDIT NANO FILE
```
nano rgym_exp/config/rg-swarm.yaml
```
# Execute the run script to start the node
```
./run_rl_swarm.sh
```


# Select Any Model of NEED

- Gensyn/Qwen2.5-0.5B-Instruct
- Qwen/Qwen3-0.6B  
- nvidia/AceInstruct-1.5B
- dnotitia/Smoothie-Qwen3-1.7B
- Gensyn/Qwen2.5-1.5B-Instruct



# Additional If using vast or Quickpot use command to Get swarm or upload 
# if using CPU use simple smtp method to quick drag and upload
`TXT USE IN AI WITH INFORMATION`
```
scp -O -P xxxPORT "/mnt/c/Users/Admin/Desktop/Gensyn/XXX/swarm.pem" xxxSSH:/workspace/rl-swarm/
```
```
scp -O -P xxxPORT "/mnt/c/Users/Admin/Desktop/Gensyn/XXX/temp-data/userApiKey.json" xxxSSH:/workspace/rl-swarm/modal-login/temp-data/
```
```
scp -O -P xxxPORT "/mnt/c/Users/Admin/Desktop/Gensyn/XXX/temp-data/userData.json.json" xxxSSH:/workspace/rl-swarm/modal-login/temp-data/
```

You may also need this if require
```
chmod 600 /workspace/rl-swarm/swarm.pem
```

Special Issue if  node crash after 10-15 hours 
```
pip install --force-reinstall trl==0.19.1
```

```
pip freeze
```

```
bash run_rl_swarm.sh
```

# If Cuda version issue
```
# For newer GPUs (RTX 40xx series, etc.)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# For older GPUs  
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

```


local tunneling 
```
sudo npm install -g localtunnel
```
```
curl https://loca.lt/mytunnelpassword
```
```
lt --port 3000
```




NANO EDIT FOR 3060

```
log_dir: ${oc.env:ROOT,.}/logs

hydra:
  run:
    dir: ${log_dir}
  job_logging:
    handlers:
      console:
        level: INFO
    root:
      level: DEBUG

training:
  max_round: 1000000
  max_stage: 1
  hf_push_frequency: 1
  num_generations: 2
  num_transplant_trees: 1
  seed: 42
  dtype: 'bfloat16'

blockchain:
  alchemy_url: "https://gensyn-testnet.g.alchemy.com/public"
  swarm_contract_address: ${oc.env:SWARM_CONTRACT,null} # This is set by modal-login in run_rl_swarm.sh
  org_id: ${oc.env:ORG_ID,null} # This is set by modal-login in run_rl_swarm.sh
  mainnet_chain_id: 685685 # currently unused, will be used with WalletSwarmCoordinator
  modal_proxy_url: "http://localhost:3000/api/"
  swarm_coordinator_abi_path: "rgym_exp/contracts/SwarmCoordinator_0.4.2.json"

eval:
  judge_base_url: "https://swarm-judge.internal-apps-central1.clusters.gensyn.ai"

prg_game_config:
  prg_game: ${oc.env:PRG_GAME,null}
  org_id: ${blockchain.org_id}
  modal_proxy_url: ${blockchain.modal_proxy_url}

communications:
  initial_peers:
    - '/ip4/38.101.215.12/tcp/30011/p2p/QmQ2gEXoPJg6iMBSUFWGzAabS2VhnzuS782Y637hGjfsRJ'
    - '/ip4/38.101.215.13/tcp/30012/p2p/QmWhiaLrx3HRZfgXc2i7KW5nMUNK7P9tRc71yFJdGEZKkC'
    - '/ip4/38.101.215.14/tcp/30013/p2p/QmQa1SCfYTxx7RvU7qJJRo79Zm1RAwPpkeLueDVJuBBmFp'

game_manager:
  _target_: rgym_exp.src.manager.SwarmGameManager
  max_stage: ${training.max_stage}
  max_round: ${training.max_round}
  log_dir: ${log_dir}
  hf_token: ${oc.env:HUGGINGFACE_ACCESS_TOKEN,null}
  hf_push_frequency: ${training.hf_push_frequency}
  run_mode: "train_and_evaluate"
  bootnodes: ${communications.initial_peers}
  prg_game_config: ${prg_game_config}
  game_state: 
    _target_: genrl.state.game_state.GameState
    round: 0
    stage: 0
  reward_manager:
    _target_: genrl.rewards.DefaultRewardManager
    reward_fn_store:
      _target_: genrl.rewards.reward_store.RewardFnStore
      max_rounds: ${training.max_round}
      reward_fn_stores:
        - _target_: genrl.rewards.reward_store.RoundRewardFnStore
          num_stages: ${training.max_stage}
          reward_fns:
            - _target_: rgym_exp.src.rewards.RGRewards
  trainer:
    _target_: rgym_exp.src.trainer.GRPOTrainerModule
    models:
      - _target_: transformers.AutoModelForCausalLM.from_pretrained
        pretrained_model_name_or_path: ${oc.env:MODEL_NAME, ${gpu_model_choice:${default_large_model_pool},${default_small_model_pool}}} 
    config:
      _target_: genrl.trainer.grpo_trainer.GRPOTrainerConfig
      dtype: ${training.dtype}
      epsilon: 0.2
      epsilon_high: 0.28
      num_generations: ${training.num_generations}
      enable_gradient_checkpointing: true
      
    log_with: wandb
    log_dir: ${log_dir}
    judge_base_url: ${eval.judge_base_url}
  data_manager:
    _target_: rgym_exp.src.data.ReasoningGymDataManager
    yaml_config_path: "rgym_exp/src/datasets.yaml"
    num_train_samples: 1
    num_evaluation_samples: 0
    num_generations: ${training.num_generations}
    system_prompt_id: 'default'
    seed: ${training.seed}
    num_transplant_trees: ${training.num_transplant_trees}
  communication:
    _target_: genrl.communication.hivemind.hivemind_backend.HivemindBackend
    initial_peers: ${communications.initial_peers}
    identity_path: ${oc.env:IDENTITY_PATH,null}
    startup_timeout: 120
    beam_size: 20
  coordinator:
    _target_: rgym_exp.src.coordinator.ModalSwarmCoordinator
    web3_url: ${blockchain.alchemy_url}
    contract_address: ${blockchain.swarm_contract_address}
    org_id: ${blockchain.org_id}
    modal_proxy_url: ${blockchain.modal_proxy_url}
    swarm_coordinator_abi_json: ${blockchain.swarm_coordinator_abi_path}

default_large_model_pool: []

default_small_model_pool:
  - Gensyn/Qwen2.5-0.5B-Instruct
```
