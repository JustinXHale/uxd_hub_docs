author: Justin X. Hale, Red Hat UXD

# cli-guidelines | UXD Hub
Command Line Interface Guidelines (CLI Guidelines)
--------------------------------------------------

Overview
--------

### UX Goal

Creating New Customers

### Effort

The goal is to enhance ROSA CLI usability, following industry-standard conventions like verb-noun structure, providing a seamless and intuitive experience for users in managing Red Hat OpenShift clusters on AWS.

### Desired user outcomes

The purpose of these guidelines is to assist in creating a consistent experience for our users across Red Hat. In doing that we must ensure our teams are on the same page when adding and creating features for our CLI. The motivation behind this is to document a set of guidelines for ROSA and OCM CLIs.

### How this effort is impact the goal

Well-documented, user-friendly CLI tools increase the likelihood of attracting and retaining ROSA customers, as they can readily and confidently manage their infrastructure, reducing the appeal of other options.

### Measuring the effort  
 

*   Usability testing - TBD
*   Analytics - TBD

* * *

Naming Conventions
------------------

*   Verb-noun structure command structure (aligns with kubectl)  
     
    *   Verb: Represents the action or operation that the command performs.
    *   Noun: Indicates the target or object on which the action is performed.

### Examples

Here, the command follows a clear Verb-Noun structure. 'Create' represents the action, and 'cluster' signifies the target object.

`rosa create cluster`

`rosa delete project`

`rosa scale nodepool`

Long format naming  
 
----------------------

Long-form options are named descriptively to indicate their purpose clearly

Example: 
---------

The long-form option is designed to explicitly convey the purpose of enabling autoscaling for a specific cluster or resource.

`--enable-autoscaling` 

`--set-min-replicas`

`--assign-admin-privileges`

Short-form option
-----------------

*   Consider only using short commands for highly used, top-level commands.
*   Commands should be 1-2 characters long

### Example: 

In this instance, 'list-clusters' is a moderately used command, and the -c flag limits the displayed clusters. Shortening the command increases efficiency without sacrificing clarity.

`rosa list-clusters -c 50`

`<span style="color: var(--ee-text-primary); font-size: 13.6px; font-weight: inherit;">rosa update user -p</span>`

`<span style="color: var(--ee-text-primary); font-size: 13.6px; font-weight: inherit;">rosa status -v</span>`

* * *

Help Flag Overview
------------------

ROSA Help Text Example Doc

*   Parts of the help flag
    *   A description of what it does
    *   Example if necessary
    *   Description of flags, if less than 3
    *   An instruction to pass the **\--help** flag for more information.
    *   Link to documentation
    *   Show full help when **\-h** and **\--help** is passed.
*   Conciseness  
     
    *   Description: Provide a brief overview of the command's functionality or purpose. Ensure clarity by summarizing its primary action.
    *   Example (if necessary):  Supplement the description with an example to illustrate command usage in a real or hypothetical scenario, aiding user understanding.
*   Flag Description
    *   If the command utilizes fewer than 3 flags:
        *   Offer a concise description of each flag, detailing its purpose or how it modifies the command's behavior. Prioritize clarity in flag explanations.
*   Instruction for Additional Information
    *   Encourage users to pass the **\--help** flag for more comprehensive details about the command and its options. This flag should provide an extended explanation or a detailed breakdown of available functionalities.
    *   Include a direct link or reference to comprehensive documentation related to the command. This directs users to more in-depth resources for a deeper understanding or additional examples.
*   Full Help Display
    *   Passing **\-h** or **\--help** triggers a complete display of help documentation, providing users with an exhaustive explanation of command usage, flags, and additional details.

Help Flag Example
-----------------

[List of ROSA Flags](https://docs.google.com/spreadsheets/d/1t2dBC5ZClaTqCAJNN2OI4sVxSMc5N0aNG4CP1TB-E7c/edit?gid=1564354721#gid=1564354721)

`$ rosa create cluster --help Usage: rosa create cluster <cluster_name> [flags] Description:Create a new Kubernetes cluster on Red Hat OpenShift Service on AWS (ROSA). Example: To create a cluster named my-cluster: rosa create-cluster my-cluster Flags: --region string Specifies the AWS region for cluster deployment. --version string Sets the ROSA version for the new cluster.< br>Additional Information: For more details and advanced options, use the --help flag with this command, like: rosa create-cluster --help Find comprehensive documentation at: <a href="https://rosa.redhat.com/docs/">https://rosa.redhat.com/docs/</a> Full Help Display: Passing -h or --help triggers a complete display of help documentation, providing exhaustive explanations of command usage, available flags, and additional details.`

* * *

Input
-----

Flags
-----

_Flags are named parameters, denoted with a hyphen and a single-letter name (-r) or a double hyphen and a multiple-letter name (--recursive). They may or may not also include a user-specified value (--file foo.txt, or --file=foo.txt). The order of flags, generally speaking, does not affect program semantics._

**Flags are preferred to args**. They involve a bit more typing, but make the use of the CLI clearer. This also allows the user to specify the flags in any order, and gives them the confidence that they are running the command correctly. 

Example
-------

`rosa create cluster --name my-cluster`

`rosa create cluster --region us-west-2 --workers 3 --instance-type m5.large --private-link`

Arguments
---------

_Arguments, or args, are positional parameters to a command. For example, the file paths you provide to cp are args. The order of args is often important: cp foo bar means something different from cp bar foo._

### Examples

`rosa create cluster my-cluster`

`rosa create cluster us-west-2 3 m5.large`

* * *

Error Messaging
---------------

*   **Make errors readable for humans**
    *   Ex: “Can't write to file.txt. You might need to make it writable by running ‘<cmd>’”
*   Important stuff last because it is the first place users will look
*   Unexpected or unexplainable errors should provide debug and traceback information, and instructions to submit a bug.

### Error Messaging Example

`rosa delete cluster my-cluster Error: Unable to delete cluster ‘my-cluster’ Reason: The cluster cannot be deleted because it still has active workloads. Solution: - Ensure there are no running applications or workloads on the cluster. - If you want to force-delete the cluster, use the following command: rosa delete cluster my-cluster --force Note: Force-deleting a cluster will terminate all workloads without proper scaling down. It's recommended to gracefully scale down and remove workloads before force-deleting.`

### Short correction message

`Error: Can't write to file.txt. You might need to make it writable by running '<cmd>'`

Corrective messaging
--------------------

While not a substitute for comprehensive error messages, consider implementing auto-correction mechanisms where applicable. Auto-correction features can enhance the user experience by automatically resolving common issues or suggesting corrections:

`Did you mean: 'rosa create cluster --name my-cluster'?`

* * *

Interactive Mode
----------------

Interactive Prompts
-------------------

Interactive prompts are questions posed to users during command execution in interactive modes. Users have the option to type "?" to access help text related to the specific question, aiding them in making informed decisions.

Features of Interactive Prompts:
--------------------------------

*   **User-Friendly Exploration**: Users can explore available options and get context-specific help during the interaction.
*   **Question Format**: Each prompt is presented as a question, guiding users to provide necessary information.

Utilizing "?" for Help:
-----------------------

**Usage**: Users can type "?" when prompted to display additional help text..

Points to remember:

*   **Clarity**: Ensure that the user understands the purpose and impact of the option.
*   **Conciseness**: Keep the text brief and to the point. Users should be able to quickly grasp the information.
*   **Consistency**: Follow a consistent style across all help texts for a cohesive user experience.
*   **User Perspective**: Frame the information from the user's perspective, focusing on what the option does and why it matters.
*   **Avoid Jargon**: Minimize technical jargon and use plain language where possible.
*   Provide a unique name for your cluster. This name will be used to create a specific web address for your cluster on openshiftapps.com.: use 

Example Interactive Prompt
--------------------------

`Allows automated deployment and management, streamlining tasks and relieving users from manual control plane handling.<b></b> <b>?</b> <b>Deploy</b> <b>cluster</b> <b>with</b> <b>Hosted</b> <b>Control</b> <b>Plane: [? for help]</b> <b>(y/N)</b>`

Actions that can’t change
-------------------------

*   Example: cluster name
*   Should we inform the user when the prompt appears?
    *   How does this affect the user when all prompts are shown?

When to add to interactive mode prompts
---------------------------------------

*   Is it a requirement or feature?
*   Does the user need to complete in order to proceed?
*   Can it only be added at creation?

Example
-------

[ROSA Quick Cluster Creation Prototype](https://www.figma.com/proto/4kQseBVRUxeyhfkGM2FKgY/ROSA-CLI?node-id=18-19&t=IIRkXUSMfRvITXbq-0&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=1%3A299&show-proto-sidebar=1)

* * *

List
----

To streamline user experience and ensure clarity, ROSA CLI follows specific guidelines for presenting version information. Prioritizing the default version for immediate user visibility and simplifying the display, this structured format enhances user understanding and navigation.

Version Display Format
----------------------

*   Prioritize the default version at the top.
*   Display "Yes" for options where there are only two choices (e.g., "Yes" and "No") for clarity.
*   Apply pagination after listing 7 versions.
*   Apply pagination after listing 5 available upgrades.

`VERSION DEFAULT UPGRADES 4.14.0 Yes 4.14.1, 4.14.2, 4.14.3, 4.14.4, 4.14.5, ... 4.13.28 4.13.29, 4.13.30, 4.13.31, 4.13.32, 4.13.33, ... 4.13.26 4.13.27, 4.13.28, 4.13.29, 4.13.30, 4.13.31, ... 4.13.25 4.13.26, 4.13.27, 4.13.28, 4.13.29, 4.13.30, ... 4.13.24 Yes 4.13.25, 4.13.26, 4.13.27, 4.13.28, 4.13.29, ... 4.13.23 4.13.24, 4.13.25, 4.13.26, 4.13.27, 4.13.28, ... ...`

* * *

Default values for prompts
--------------------------

Option prompts, where users choose from a list of predefined options, often involve the consideration of default values. This section provides guidelines on how to handle default values for option prompts effectively.

Clarity in Defaults:
--------------------

*   Default values should be clearly communicated to the user during interactive prompts.
*   Ensure that default values are selected based on meaningful criteria, such as common use or logical defaults.

First in the List:
------------------

*   Be cautious when selecting the first option in the list as the default value.
*   In some cases, the first option might not represent a true default but merely the initial entry in the list.

Optional vs. Default:
---------------------

*   Distinguish between prompts that are optional and those with predefined default values.
    *   **Optional** prompts may not have a default value
    *   **Default** values are automatically applied if the user doesn't provide input.

Implementation
--------------

*   Clearly indicate default values:
    *   Square brackets next to the option prompt.
    *   Parentheses next to the option prompt.
    *   Lower and uppercase lettering for yes/no option. Ex: (y/N)
*   Inform users that the default value is automatically selected if they proceed without providing explicit input.
*   Include information on how to accept the default value or change it during the interaction.

`OIDC Configuration ID (default = '26vf33ho0t7bpu29cosplrokcp7n7l0e | <a href="https://d3gt1gce2zmg3d.cloudfront.net/26vf33ho0t7bpu29cosplrokcp7n7l0e">https://d3gt1gce2zmg3d.cloudfr...</a>'): [Use arrows to move, type to filter, ? for more help]`

* * *

Documenting Flags
-----------------

Flags play a crucial role in defining the behavior of a command. Proper documentation ensures that users understand each flag's purpose, making the CLI more accessible and user-friendly.

Descriptive Naming
------------------

*   Choose flag names that clearly convey their purpose.
*   Prefer long-form flags for better readability and understanding.

### Example

`--enable-autoscaling`

Short-form Flags:
-----------------

*   If providing short-form flags, keep them concise and easy to remember.
*   Short-form flags are optional but can enhance efficiency.

`-a, --autoscale`

### Flag syntax

*   Define the syntax for each flag including any required parameters.

`--region <region_name>`

### Boolean Flags

State whether a flag is boolean (true/false) and its default behavior.

`--force # Boolean flag, default is false`

### Concise descriptions

Provide a brief description of the flags purpose.

`--region string Specifies the AWS region for cluster deployment.`

### Default Values

Indicate if a flag has a default value.

`--version string Sets the ROSA version for the new cluster. (Default is the latest version)`

<<<<<<< HEAD
* * *
=======
* * *
>>>>>>> 55d69df (cli guidelines markdown)
