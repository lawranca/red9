Pro_Pack : project
=======================

	>>> # main import statement to use
	>>> from Red9.pro_pack import PROJECT_DATA
	
Red9 ProPack Project object (python object) is bound on boot to pro_pack itself and is used to manage data against a given clients 
project file. The project file itself is a simple Json formatted file usually found under the client resource folder here:

/Red9_ClientCore/xxxx/resource/xxx.project

We also recently added new Maya environment variable to expand the use of this data for those running ProPack without a ClientCore module.
If you enter the following into your Documents/maya/20XX/maya.env then all xxx.project files found will be added to the list of avaiable projects,
enabling you to switch all of the Project data on the fly inside Maya via either the ToolShelf button, or in the Red9 ProjectManager UI.
This also means that these projects can be on a mapped network drive so all users point to the same data.

RED9_PROJECT_RESOURCES=D:/my_project_path

If your build doens't have the Red9_ClientCore modules or the above variable then you'll find an empty project file here:

Red9/pro_pack/resources/empty.project

The project data can now be edited directly from the Red9 ProjectManagerUI rather than having to edit the JSON data directly. This saves
unexperienced users from badly formatting the JSON file and breaking the pipelines! You can also now create new projects from the UI.

The code that adds and physically binds new project files to the systems is as follows. These lines must be added to your __init__ or
somewhere thats parsed on boot so they're bound to the PROJECT_DATA object. You can add as many of these as are required
for your studios setups. Each additional project added will show up in the ProjectManager UI in Maya for you to switch between on the fly.

	>>> from Red9.pro_pack import PROJECT_DATA
	>>> # to bind a single new project file
	>>> PROJECT_DATA.add_project(os.path.join(get_resources(), 'my_new_game.project'))
	>>>
	>>> # to add ALL projects found under a given folder
	>>> PROJECT_DATA.add_projects_found(folder)

The PROJECT_DATA object generated is used anywhere in the Red9 codebase where a path is required which may be specific to a project, 
or folder mount, allowing us to tailor setups to a clients needs without modifying the underlying codebase at all. The project file 
is JSON formatted and bound to the PROJECT_DATA on load, this means that anything in that .project file will be exposed as an attribute 
directly on the PROJECT_DATA.data object.

	>>> # for example
	>>> from Red9.pro_pack import PROJECT_DATA
	>>> PROJECT_DATA.data.fps  # project frame rate
	>>> PROJECT_DATA.data.p4_root  # projects p4_root path which is dynamically found
	>>> 
	>>> # theres also a pretty print function 
	>>> PROJECT_DATA.pretty_print()
	
Note that PROJECT also manages any initial P4 bindings and therefore exposes some of the root paths directly on the Project object. 
Because of this many of the base paths we may setup in the .project file maybe prefixed with p4_root+

	>>> "pose_projectpath": "p4_root+/Data/Animations_Source/Characters/Human/_projectposes/red9/", 

This denotes that the path, in this case the pose_projectpath, is dynamic to the users current P4 workspace mount, we replace the p4_root in this
case with the actual p4_root found in the users system.

Note that on boot we check if source control is available and setup the systems accordingly, all our P4 management codebase, found in the 
importexport module, relies on the PROJECT_DATA.source_control() call, which in itself is setup at boot. This function must return 'perforce'
if any of the systems are to run under the p4 api natively. This NOT limited to P4 and is done in this manner to allow us to bind other source control
systems to the r9File objects dynamically.


.. automodule:: Red9.pro_pack.core.project

   .. rubric:: Main Classes

   .. autosummary::
   	  
      ProjectBase
      Project




    


   
   
   