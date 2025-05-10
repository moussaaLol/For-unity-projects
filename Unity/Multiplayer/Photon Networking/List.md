# Common Photon PUN Variables, Components, and Functions

This reference covers frequently used elements when working with Photon PUN for networking in Unity.

## Core Classes & Static Properties

* **`PhotonNetwork`**:
    * **Description**: The main static class for managing the connection to the Photon network, rooms, lobbies, and general network state.
    * **Common Properties/Methods**:
        * `PhotonNetwork.ConnectUsingSettings()`: Connects to the Photon Cloud using the settings configured in the PhotonServerSettings asset.
        * `PhotonNetwork.JoinLobby()`: Joins the default lobby.
        * `PhotonNetwork.CreateRoom(string roomName)`: Creates a new room.
        * `PhotonNetwork.JoinRoom(string roomName)`: Joins an existing room.
        * `PhotonNetwork.JoinRandomRoom()`: Joins a random available room.
        * `PhotonNetwork.LeaveRoom()`: Leaves the current room.
        * `PhotonNetwork.LoadLevel(string levelName)`: Loads a scene across the network for all players in the room.
        * `PhotonNetwork.IsConnected`: (bool) True if connected to the Photon server.
        * `PhotonNetwork.InLobby`: (bool) True if currently in a lobby.
        * `PhotonNetwork.InRoom`: (bool) True if currently in a room.
        * `PhotonNetwork.CurrentRoom`: (Room) Reference to the current room object.
        * `PhotonNetwork.LocalPlayer`: (Player) Reference to the local client's player object.
        * `PhotonNetwork.PlayerList`: (Player[]) An array of all players currently in the room.
        * `PhotonNetwork.MasterClient`: (Player) Reference to the master client's player object.
        * `PhotonNetwork.Instantiate(string prefabName, Vector3 position, Quaternion rotation, byte group)`: Instantiates a network-synced GameObject. `prefabName` must be in a "Resources" folder.

* **`PhotonView`**:
    * **Description**: A component attached to a GameObject that identifies it across the network and allows for network synchronization and RPC calls. Every network-synced GameObject needs one.
    * **Common Properties**:
        * `photonView.IsMine`: (bool) True if this PhotonView belongs to the local client.
        * `photonView.ViewID`: (int) A unique identifier for this PhotonView across the network.
        * `photonView.Owner`: (Player) The player who owns this PhotonView.
    * **Common Methods**:
        * `photonView.RPC(string methodName, RpcTarget target, params object[] parameters)`: Calls a method on other clients or the server.

* **`Player`**:
    * **Description**: Represents a player in the network room. Can be the local player or a remote player.
    * **Common Properties**:
        * `player.ActorNumber`: (int) Unique ID for the player within the room.
        * `player.NickName`: (string) The player's display name.
        * `player.IsMasterClient`: (bool) True if this player is the master client.
        * `player.IsLocal`: (bool) True if this player is the local client.

* **`Room`**:
    * **Description**: Represents a room on the Photon server where players gather to play together.
    * **Common Properties**:
        * `room.Name`: (string) The name of the room.
        * `room.PlayerCount`: (int) The current number of players in the room.
        * `room.MaxPlayers`: (byte) The maximum number of players allowed in the room.
        * `room.IsOpen`: (bool) Whether the room is open for new players to join.
        * `room.IsVisible`: (bool) Whether the room is visible in the lobby room list.
        * `room.Players`: (Dictionary<int, Player>) A dictionary of all players in the room, keyed by ActorNumber.

## Interfaces

* **`IPunObservable`**:
    * **Description**: An interface implemented by scripts attached to a `PhotonView` to synchronize data regularly (e.g., position, rotation).
    * **Required Method**:
        * `OnPhotonSerializeView(PhotonStream stream, PhotonMessageInfo info)`: This method is called by the `PhotonView` to serialize (write) and deserialize (read) data to/from the network stream.

## Callbacks (from `MonoBehaviourPunCallbacks`)

* **`MonoBehaviourPunCallbacks`**:
    * **Description**: A base class you can inherit from to easily receive common PUN callbacks (events).
    * **Common Callbacks**:
        * `OnConnectedToMaster()`: Called when the client is connected to the Master Server and ready for matchmaking.
        * `OnJoinedLobby()`: Called when the client has joined a lobby.
        * `OnJoinedRoom()`: Called when the client has successfully joined a room.
        * `OnCreatedRoom()`: Called when the client has successfully created a room.
        * `OnLeftRoom()`: Called when the client has left a room.
        * `OnDisconnected(DisconnectCause cause)`: Called when the client is disconnected from the server.
        * `OnPlayerEnteredRoom(Player newPlayer)`: Called when another player enters the room.
        * `OnPlayerLeftRoom(Player otherPlayer)`: Called when another player leaves the room.
        * `OnMasterClientSwitched(Player newMasterClient)`: Called when the master client leaves the room and a new one is assigned.
        * `OnRoomListUpdate(List<RoomInfo> roomList)`: Called when the list of rooms in the lobby is updated.

## Remote Procedure Calls (RPCs)

* **`[PunRPC]`**:
    * **Description**: An attribute placed above a method definition to mark it as a method that can be called remotely via a `PhotonView`.
    * **Example Usage**:
        ```csharp
        [PunRPC]
        void TakeDamage(int damageAmount)
        {
            damagePlayer(damageAmount);
        }
        ```

* **`RpcTarget`**:
    * **Description**: An enum used with `photonView.RPC()` to specify which clients should receive the RPC call (e.g., `All`, `Others`, `MasterClient`, `AllBuffered`).

## Synchronization Modes (`PhotonView.Synchronization`)

* **`Off`**: No synchronization is performed by the PhotonView itself. You handle everything manually (e.g., via RPCs).
* **`ReliableDeltaCompressed`**: Synchronizes the state of observed components (those implementing `IPunObservable`) reliably, sending only changes since the last send.
* **`Unreliable`**: Synchronizes the state of observed components unreliably, sending the full state each time. Faster but data might be lost.
* **`UnreliableOnChange`**: Synchronizes the state of observed components unreliably, sending data only when changes are detected.

## Other Useful Concepts

* **`PhotonStream`**:
    * **Description**: An object used in `OnPhotonSerializeView` to write data to or read data from the network.
    * **Common Methods**:
        * `stream.SendNext(object obj)`: Writes an object to the stream.
        * `stream.ReceiveNext()`: Reads the next object from the stream.
        * `stream.IsWriting`: (bool) True if the stream is being written to (serializing).
        * `stream.IsReading`: (bool) True if the stream is being read from (deserializing).

* **`PhotonMessageInfo`**:
    * **Description**: Provides information about a received network message (e.g., the sender, the timestamp). Used in `OnPhotonSerializeView` and RPC methods.

* **`RoomOptions`**:
    * **Description**: A class used to configure settings when creating a room (e.g., max players, visibility, custom properties).

* **`TypedLobby`**:
    * **Description**: Represents a specific lobby type. Used when joining or creating rooms in different lobbies.
      
**According to Photon Networking documentation.**
