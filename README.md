# SLIMTutorial2024
Tutorials for 2024 ML4Seismic meeting

## Step by step guide

1. Copy and paste the IP address `34.123.253.131` to a brower.
2. Enter your user name and create a password the first time you log in.
3. All tutorials are saved under the directory  `/opt/ml4seismic_tutorial` (read only). Each user should make a personal copy of the tutorial directory to the home directory to work with:
  ```bash
  cp -r /opt/ml4seismic_tutorial/ ~/ml4seismic_tutorial/
  ```
  Each tutorial has its own environment files `Project.toml` and `Manifest.toml`. 
   ```bash 
   /opt/ml4seismic_tutorial/
   ├── tutorial_1/
   │   ├── Project.toml
   │   ├── Manifest.toml
   │   └── tutorial_notebook.ipynb
   ├── tutorial_2/
   │   ├── Project.toml
   │   ├── Manifest.toml
   │   └── tutorial_notebook.ipynb
   ```
4. Open a terminal and run `julia -e 'using Pkg; Pkg.add("IJulia")`'. This installs a Julia kernel to the Jupyter notebook. <p style="color:red;">**TODO**: Can I do this for everyone?</p>
5. `cd` into the directory of one of the tutorial and run the following commands to build the environment.
  ```julia
  using Pkg
  Pkg.activate(".")
  Pkg.instantiate() # Install the dependencies
  Pkg.add(["Dependency1", "Dependency2", ...]) # Install more packages if necessary
  ```


## Notes

1. Don't use `kubernetes`
2. Sometimes Jupyterhub needs to restart itself.
3. Try to install Julia packages for everyone.

# Notes on setting up Jupyterhub
1.
2. Add path of Julia/Juliap to `/etc/profile`
```bash
echo 'export PATH="/opt/juliaup/bin:$PATH"' | sudo tee -a /etc/profile
```
3. Permissions: Make sure the `/opt/juliaup` directory and its contents have appropriate permissions for read and execute access by all users:
```bash
sudo chmod -R 755 /opt/juliaup
```

