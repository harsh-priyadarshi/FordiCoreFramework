## Welcome to the FordiCoreFramework wiki!

### Compatibility
1. Unity Version: Unity 2019.4.1f1 (64-bit) and above

### Setup
1. Install TextMesh Pro.
2. Install [ Unity-UI-Rounded-Corners](https://github.com/kirevdokimov/Unity-UI-Rounded-Corners/releases/tag/v2.0.1).
2. Import FordiCoreFramework_v1.1.0
3. To use an existing scene as an experience scene drop  Fordi > Prefabs > StateMachine.prefab in it.

### Populating Experience Specific Menu
1. Open Fordi > Prefabs > StateMachine.prefab.
2. Select the particular experience gameobject found under Experiences.
3. Select the menu asset.
3. Click on the plus button to add a new menu item. Menus can be reordered.

4. Expand the item list and assign menu name, text, command (optional), path (optional) and sprite (optional) for each item.

### Local Database

1. Asset database file is available at Fordi > UIFramework > Settings > AssetDatabase.asset.
2. You can also create new database file by right-clicking in Project section. You must assign the new file in CommonResource.cs script attached to StateMachine.prefab.

### User Interface Customisation

The user interface is fully customizable. The colors can be customized globally by tweaking the default theme settings.
1. This can also be used to create multiple themes for the app.
2. Apart from this, blueprints of all screens and their list item are available as prefabs and these can be customized.
3. To override the global theme, create a theme, and assign as skin to individual UI elements.


### Scripting API
* **`IOC.cs`:** This is the container class having all core references.
1. `public static T Resolve<T>()` : This can be used to fetch references.

* **`ExperienceDeps.cs`:** This class is mainly used to register dependencies.

* **`MenuSelection.cs`:** This is used to store all resources selected by user while starting a level. It can be accessed as:
`IMenuSelection m_menuSelection = IOC.Resolve<MenuSelection>();`

* **`StateMachine.cs`:** This is the main class managing core functionality and different experiences. It should be accessed as:
`IStateMachine m_stateMachine = IOC.Resolve<IStateMachine>();`
1. `IStateMachine.CurrentExperience`: It holds the current experience that is running.

* **`Experience.cs`:** This is the class that represents each kind of experience.
1. `OnLoad`: This function is called when an experience is loaded. It can be used to do all shorts of initialisation.
2. `UpdateResourceSelection`: This function is called every time a resource is selected from menu. It should be used to do operations on new resource that was selected from menu.
3. `ExecuteMenuCommand`: This function is called every time a menu item is clicked.

* **`UIEngine.cs`:** This class is responsible for managing the user interface. It can be accessed as:
`IUIEngine` m_uiEngine = IOC.Resolve<IUIEngine>();`
It can be used to open and close menus and check if menu is open:
1. `void OpenMenu(MenuArgs args);`
2. `void CloseMenu();`
3. `void OpenGridMenu(MenuArgs args);`
4. `bool IsMenuOpen { get; }`

* **Create New Experience:**
1. Create a class `NewExperience.cs` extending `Experience.cs`.
2. Extend the `ExperienceType` enum to include a type for this new experience. Make sure you don't change the order of types.
3. Create an empty gameobject inside Experiences of ExperieinceMachine prefab and attach the `NewExperience.cs` on it.
4. Create a new variable inside `StateMachine.cs` for this new experience. Initialize it in `Awake` and update `GetExperience` methods.

```C#
        public IExperience GetExperience(ExperienceType experience)
        {
            switch (experience)
            {
                case ExperienceType.HOME:
                    return m_home;
                default:
                    return null;
            }
        }

        public IExperience GetExperience(ExperienceType experience)
        {
            switch (experience)
            {
                case ExperienceType.HOME:
                    return m_home;
                default:
                    return null;
            }
        }
```
**Note:** For all new updates it is advised to create new scripts instead of changing the package scripts.
