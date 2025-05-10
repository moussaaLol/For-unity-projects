# Common PlayFab Reference (C# & JS)

This document provides a reference for fundamental PlayFab concepts and common API calls, with examples in both C# (for Unity) and JavaScript (for web or Node.js).

**Core PlayFab Concepts:**

* **Title ID**: Your unique identifier for your game/title in PlayFab. Required for most API calls.
* **Entity**: Represents a player, group, character, or other entity in PlayFab. Identified by an Entity ID and Entity Type.
* **Player**: A specific type of Entity representing a human player. Identified by a PlayFab ID.
* **API Calls**: Requests made to PlayFab servers to perform actions (e.g., login, get data, update currency).
* **Callbacks**: Functions that are executed when an API call completes, either successfully or with an error.

**PlayFab API Classes (C# - Unity):**

* **`PlayFabClientAPI`**: Used for API calls made from the client (e.g., Unity game client). These calls are typically authenticated by a player login.
* **`PlayFabServerAPI`**: Used for API calls made from a trusted server environment (e.g., your game server, an Azure Function). These calls require a Secret Key and are not tied to a specific player's authentication.
* **`PlayFabAdminAPI`**: Used for administrative tasks, typically from development tools or backend scripts, requiring a Secret Key. Not for use in client builds.

**PlayFab API Calls (Common Examples):**

---

### 1. Login

This section shows how to authenticate a player using `LoginWithCustomID`.

**C# Example (Unity - Client)**

```csharp
using PlayFab;
using PlayFab.ClientModels;
using UnityEngine;

public class PlayFabLogin : MonoBehaviour
{
    private string playFabTitleId = "G7H8J";
    private string customPlayerId = "Player12345"; 

    public void Start()
    {
        if (string.IsNullOrEmpty(PlayFabSettings.TitleId))
        {
            PlayFabSettings.TitleId = playFabTitleId;
        }

        var request = new LoginWithCustomIDRequest {
            CustomId = customPlayerId,
            CreateAccount = true
        };

        PlayFabClientAPI.LoginWithCustomID(request, OnLoginSuccess, OnLoginFailure);
    }

    private void OnLoginSuccess(LoginResult result)
    {
        Debug.Log("Successfully logged in!");
    }

    private void OnLoginFailure(PlayFabError error)
    {
        Debug.LogError($"Login failed: {error.ErrorMessage}");
    }
}
```

**JavaScript Example (Web/Node.js)**

```javascript
/* For web: Include the PlayFab SDK script: <script src="[https://unpkg.com/playfab-sdk@2.x/dist/PlayFab/PlayFabClientSdk.js](https://unpkg.com/playfab-sdk@2.x/dist/PlayFab/PlayFabClientSdk.js)"></script>
For Node.js: npm install playfab-sdk
const PlayFab = require("playfab-sdk/Scripts/PlayFab/PlayFab");
const PlayFabClient = require("playfab-sdk/Scripts/PlayFab/PlayFabClient");*/
var playFabTitleIdJS = "K1L2M"; 
var customPlayerIdJS = "JSUser9876"; 

PlayFab.settings.titleId = playFabTitleIdJS;

var loginRequest = {
    CustomId: customPlayerIdJS,
    CreateAccount: true
};

PlayFabClient.LoginWithCustomID(loginRequest, (error, result) => {
    if (error) {
        console.error("Login failed:", error.errorMessage);
    } else {
        console.log("Successfully logged in!");
    }
});
```

---

### 2. Get Player Data

This section shows how to retrieve custom data stored for the logged-in player.

**C# Example (Unity - Client)**

```csharp
using PlayFab;
using PlayFab.ClientModels;
using UnityEngine;

public class PlayFabGetData : MonoBehaviour
{
    public void GetPlayerData()
    {
        PlayFabClientAPI.GetUserData(new GetUserDataRequest(), OnGetDataSuccess, OnGetDataFailure);
    }

    private void OnGetDataSuccess(GetUserDataResult result)
    {
        Debug.Log("Got Player Data:");
        if (result.Data == null || result.Data.Count == 0)
        {
            Debug.Log("No data found for this player.");
        }
        else
        {
            foreach (var item in result.Data)
            {
                Debug.Log($"Key: {item.Key}, Value: {item.Value.Value}");
            }
        }
    }

    private void OnGetDataFailure(PlayFabError error)
    {
        Debug.LogError($"Get Data failed: {error.ErrorMessage}");
    }
}
```

**JavaScript Example (Web/Node.js)**

```javascript
PlayFabClient.GetUserData({}, (error, result) => {
    if (error) {
        console.error("Get Data failed:", error.errorMessage);
    } else {
        console.log("Got Player Data:");
        if (!result.data.Data || Object.keys(result.data.Data).length === 0) {
            console.log("No data found for this player.");
        } else {
            for (const key in result.data.Data) {
                if (result.data.Data.hasOwnProperty(key)) {
                    console.log(`Key: ${key}, Value: ${result.data.Data[key].Value}`);
                }
            }
        }
    }
});
```

---

### 3. Update Player Data

This section shows how to update custom data for the logged-in player.

**C# Example (Unity - Client)**

```csharp
using PlayFab;
using PlayFab.ClientModels;
using UnityEngine;
using System.Collections.Generic;

public class PlayFabUpdateData : MonoBehaviour
{
    public void UpdatePlayerData()
    {
        var request = new UpdateUserDataRequest {
            Data = new Dictionary<string, string> {
                {"PlayerLevel", "15"},
                {"LastActivity", System.DateTime.UtcNow.ToString()}
            }
        };

        PlayFabClientAPI.UpdateUserData(request, OnUpdateDataSuccess, OnUpdateDataFailure);
    }

    private void OnUpdateDataSuccess(UpdateUserDataResult result)
    {
        Debug.Log("Successfully updated player data.");
    }

    private void OnUpdateDataFailure(PlayFabError error)
    {
        Debug.LogError($"Update Data failed: {error.ErrorMessage}");
    }
}
```

**JavaScript Example (Web/Node.js)**

```javascript
var updateRequest = {
    Data: {
        "PlayerLevel": "15",
        "LastActivity": new Date().toISOString()
    }
};

PlayFabClient.UpdateUserData(updateRequest, (error, result) => {
    if (error) {
        console.error("Update Data failed:", error.errorMessage);
    } else {
        console.log("updated player data.");
    }
});
```

---

### 4. Get Virtual Currency

This section shows how to retrieve the player's virtual currency balances.

**C# Example (Unity - Client)**

```csharp
using PlayFab;
using PlayFab.ClientModels;
using UnityEngine;

public class PlayFabGetCurrency : MonoBehaviour
{
    public void GetCurrency()
    {
        PlayFabClientAPI.GetUserInventory(new GetUserInventoryRequest(), OnGetInventorySuccess, OnGetInventoryFailure);
    }

    private void OnGetInventorySuccess(GetUserInventoryResult result)
    {
        Debug.Log("Got User Inventory:");
        if (result.VirtualCurrency == null || result.VirtualCurrency.Count == 0)
        {
            Debug.Log("No virtual currency found.");
        }
        else
        {
            foreach (var vc in result.VirtualCurrency)
            {
                Debug.Log($"Currency Code: {vc.Key}, Balance: {vc.Value}");
            }
        }
    }

    private void OnGetInventoryFailure(PlayFabError error)
    {
        Debug.LogError($"Get Inventory failed: {error.ErrorMessage}");
    }
}
```

**JavaScript Example (Web/Node.js)**

```javascript
PlayFabClient.GetUserInventory({}, (error, result) => {
    if (error) {
        console.error("Get Inventory failed:", error.errorMessage);
    } else {
        console.log("Got User Inventory:");
        if (!result.data.VirtualCurrency || Object.keys(result.data.VirtualCurrency).length === 0) {
            console.log("No virtual currency found.");
        } else {
            for (const code in result.data.VirtualCurrency) {
                if (result.data.VirtualCurrency.hasOwnProperty(code)) {
                    console.log(`Currency Code: ${code}, Balance: ${result.data.VirtualCurrency[code]}`);
                }
            }
        }
    }
});
```

---

**Handling Callbacks:**

PlayFab API calls are asynchronous. You provide callback functions to handle the response.

* **Success Callback**: Receives the result object specific to the API call.
* **Error Callback**: Receives an error object containing details about the failure.

**C# Callback Signature:**

```csharp
void OnApiCallSuccess(ResultType result) { /* Handle success */ }
void OnApiCallFailure(PlayFabError error) { /* Handle error */ }
```
*(Replace `ResultType` with the specific result class for the API call, e.g., `LoginResult`, `GetUserDataResult`)*

**JavaScript Callback Signature:**

```javascript
(error, result) => {
    if (error) {
        
    } else {

    }
}
