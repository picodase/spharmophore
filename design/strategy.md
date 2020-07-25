This will include the following steps:
1. Pre-processing.
    - Take in a target structure, or ensemble/trajectory (<filename>.pdb).
    - Remove water, non-main-chain amino acids, nucleic acids, and ligands if included (non-residues).
2. Determine structure set. Select one method to extract intermediate structures for pharmacophore generation from the apo, free trajectory:
    1. **Normal mode analysis**
        1. Determine normal modes using **ProDy**.
        2. Parse these normal modes frames into a sequential PDB file for each normal mode(specify number of modes).
    2. **Markov state models**
        1. Determine Markov states using **PyEMMA**.  
        2. Parse these Markov states into a sequential PDB file for each state, ordered by energy.
    3. **Unprocessed simulation frames**
3. For each frame created, generate feature maps in the protein binding pockets.
    1. Determine the binding pocket location by specifying box dimensions and center on the protein of interest(must be input from user).
    2. Generate the protein binding pocket surface using PyMOL's surface-drawing utility.
    3. On the selected atoms, generate and map crucial ligand binding elements onto the pocket atoms as listed below:
        - H-bond (A or D)
        - Salt bridges (+ or -)
        - Pi-stacking
4. Generate ideal geometric and feature satisfiers for each generated map. Includes generating features complementary to:
    1. H-bond A/D cones
    2. Salt-bridge spheres
    3. Pi-stacking normal planes
5. Create families of structures that satisfy the features generated. Select a method below:
    1. **Homology mapping**
        1. Take in a library of ligands. Iterate through the list.
        2. Generate a sampling of meaningful (associated with the features described above) conformational space for each ligand. (done beforehand)
        3. Transform each low-energy and reasonably-low energy structure into a pharmocaphore, including features listed above.
        4. Normalize structures by rotation and translation.
        5. Compare and rank by RMSD.
        6. Return a list of structures comparing by RMSD each reasonable conformation.
    2. **Neighbor-bond growth matching**
    3. **OTHER??**
6. Format output for human-viewability.
