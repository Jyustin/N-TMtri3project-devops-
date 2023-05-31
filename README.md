Current Runtime Link: https://jyustin.github.io/N-TMtri3project-devops-/

- Running The Frontend Repository Locally. This is incredibly useful for testing purposes.
    
 
    ```ruby
    bundle exec jekyll serve -H 0.0.0.0 -P 4001
    ```
 - Cloning the Repository for Modification & Using in VSCode

    ``` git
    cd ~
    cd ./_vscode
    git clone https://github.com/Jyustin/N-TMtri3project-devops-.git
    cd ./N-TMtri3project-devops-.git
    code .
**SportsGame: Frontend**:

A frontend is necessary for this project, as it cannot simply be run through a console.

We want the user to be able to see and interact with the visualized data and sorting mechanisms for themselves and learn more about the sports that are featured in our application.

The frameworks and languages used for the frontend include the following:

1. HTML & CSS
2. JavaScript


**SportsGame: Environment Organization**:

**/data**: Metadata (YML) contained for Mario Sprite Animation

**/includes**: This contains the home.html file which organizes the top organization table in the frontend home screen. This has all the proper links to each subpage. 

**/layouts**: This has the primary default.html file which calls the home.html file to set out the theme, also setting the stylesheet information for frontend customization.

**/assets**: This contains the CSS folder, which contains the CSS stylesheet file. This is crucial for frontend customization. It is very important that file naming is adjusted based on the reference to the stylesheet in the default.html file. 

**config.yml**: This file is crucial, as it sets many basic settings. It sets the primary Jekyll theme, the title of the site to be shown, the description beneath the title shown on the frontend, and the GitHub repository link if the user would want to reference to the code.

**Files in Root Directory Pertaining to Specific Features**:

*nbabracket.md*: The primary frontend code for the NBA Bracket Feature.

*nbastats.md*: The primary frontend code for the NBA Statistics Feature.

*nflstats.md*: The primary frontend code for the NFL Statistics Feature.


**There are many PNG and test markdown files hovering in the repository. These are solely for the purpose of the sprite animation's functionality and the basketball screensaver feature, and should not be touched unless the animation or screensaver is to be removed or modified.**
