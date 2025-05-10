# Common Unity Variables, Components, and Functions

This is a quick reference for some frequently used elements in Unity scripting.

## Built-in Variables (Accessible from MonoBehaviour scripts)

* **`gameObject`**:
    * **Description**: A reference to the GameObject that this script is attached to.
* **`transform`**:
    * **Description**: A reference to the Transform component attached to this GameObject. Used for position, rotation, and scale.
* **`this.gameObject`**:
    * **Description**: Explicitly refers to the GameObject the script is attached to. Same as `gameObject`.
* **`this.transform`**:
    * **Description**: Explicitly refers to the Transform component. Same as `transform`.
* **`Time.deltaTime`**:
    * **Description**: The time in seconds it took to complete the last frame (Read Only). Useful for making movements frame-rate independent.
* **`Time.fixedDeltaTime`**:
    * **Description**: The interval in seconds at which physics and other fixed frame rate updates are performed (Read Only). Used in `FixedUpdate`.
* **`Input`**:
    * **Description**: A static class used to read input from the keyboard, mouse, joystick, and touch screens.

## Common Components

* **`Transform`**:
    * **Description**: Every GameObject has a Transform. It stores the position, rotation, and scale of the object in the scene.
* **`Rigidbody`**:
    * **Description**: Adds physics properties to a GameObject, allowing it to be affected by gravity and forces. Used for physics-based movement.
* **`Rigidbody2D`**:
    * **Description**: The 2D equivalent of Rigidbody for 2D physics.
* **`Collider`** (e.g., `BoxCollider`, `SphereCollider`, `CapsuleCollider`, `MeshCollider`):
    * **Description**: Defines the shape of an object for physics collisions. Used to detect when objects hit each other.
* **`Collider2D`** (e.g., `BoxCollider2D`, `CircleCollider2D`):
    * **Description**: The 2D equivalent of Collider for 2D physics.
* **`MeshRenderer`**:
    * **Description**: Renders the mesh of a GameObject. Requires a Mesh Filter component.
* **`SpriteRenderer`**:
    * **Description**: Renders 2D sprites in the scene.
* **`Light`** (e.g., `DirectionalLight`, `PointLight`, `Spotlight`, `AreaLight`):
    * **Description**: Adds light sources to the scene to illuminate objects.
* **`Camera`**:
    * **Description**: Captures and displays a view of the scene to the player.
* **`AudioSource`**:
    * **Description**: Plays audio clips in the scene.
* **`AudioListener`**:
    * **Description**: Acts as a microphone, receiving audio from Audio Sources. Usually attached to the main camera.
* **`Animator`**:
    * **Description**: Controls animations for a GameObject using an Animator Controller asset.
* **`Canvas`**:
    * **Description**: A UI element that contains all other UI elements (like Text, Images, Buttons).
* **`RectTransform`**:
    * **Description**: Used by UI elements instead of Transform for layout and positioning within a Canvas.

## Common Functions / Methods

* **`Awake()`**:
    * **Description**: Called when the script instance is being loaded. Happens before `Start()`. Used for initialization.
* **`Start()`**:
    * **Description**: Called on the first frame update after the script is enabled. Used for initialization that might rely on other objects being initialized.
* **`Update()`**:
    * **Description**: Called every frame. Used for most game logic, like movement, input handling, etc.
* **`FixedUpdate()`**:
    * **Description**: Called at a fixed time interval. Used for physics calculations.
* **`LateUpdate()`**:
    * **Description**: Called after all `Update` functions have been called. Useful for camera follow logic.
* **`OnTriggerEnter(Collider other)`**:
    * **Description**: Called when the collider attached to this GameObject enters a trigger collider.
* **`OnTriggerStay(Collider other)`**:
    * **Description**: Called every frame while the collider attached to this GameObject is inside a trigger collider.
* **`OnTriggerExit(Collider other)`**:
    * **Description**: Called when the collider attached to this GameObject exits a trigger collider.
* **`OnCollisionEnter(Collision collision)`**:
    * **Description**: Called when this collider/rigidbody has begun touching another rigidbody/collider.
* **`OnCollisionStay(Collision collision)`**:
    * **Description**: Called once per frame for every collider/rigidbody that is touching this rigidbody/collider.
* **`OnCollisionExit(Collision collision)`**:
    * **Description**: Called when this collider/rigidbody has stopped touching another rigidbody/collider.
* **`GetComponent<T>()`**:
    * **Description**: Retrieves a component of type `T` from the same GameObject.
* **`GetComponentInChildren<T>()`**:
    * **Description**: Retrieves a component of type `T` from the children of this GameObject.
* **`GetComponentInParent<T>()`**:
    * **Description**: Retrieves a component of type `T` from the parent of this GameObject.
* **`FindObjectOfType<T>()`**:
    * **Description**: Finds and returns the first active loaded object of Type `T`. Use sparingly as it can be slow.
* **`FindObjectsOfType<T>()`**:
    * **Description**: Finds and returns all active loaded objects of Type `T`. Use sparingly.
* **`Instantiate(Object original)`**:
    * **Description**: Clones the object `original` and returns the clone. Used to create new instances of prefabs or GameObjects.
* **`Destroy(Object obj)`**:
    * **Description**: Destroys the object `obj`.
* **`Destroy(Object obj, float t)`**:
    * **Description**: Destroys the object `obj` after `t` seconds.
* **`Debug.Log(object message)`**:
    * **Description**: Logs a message to the Unity Console. Useful for debugging.
* **`Debug.LogError(object message)`**:
    * **Description**: Logs an error message to the Unity Console.
* **`Debug.LogWarning(object message)`**:
    * **Description**: Logs a warning message to the Unity Console.

**according to the Unity Engine documentation**
