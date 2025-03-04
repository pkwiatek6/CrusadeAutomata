# Crusade Automata

Crusade Automata is an open-source backend system for managing and automating Warhammer 40K Crusade campaigns. It provides a self-hosted API for tracking armies, battles, XP progression, and battle scars while supporting Lua-based plugins for automation.

## Features

- **Self-hosted API** for managing Crusade campaigns
- **User authentication** with bcrypt and JWT
- **Battle tracking** with XP and unit updates
- **Lua-based plugin system** for automation
- **RESTful API** with JSON-based requests

## Technology Stack

- **Backend:** Go (Golang)
- **Database:** SQLite for local hosting
- **Authentication:** bcrypt + JWT
- **Plugins:** Lua (via GopherLua)
- **API Protocol:** REST

## Installation

### Prerequisites

- Go (1.20+ recommended)
- SQLite
### Clone the Repository

```
git clone https://github.com/pkwiatek6/crusade-automata.git
cd crusade-automata
```

### Configure Environment

Create a `.env` file with database and authentication settings:

```
DB_TYPE=sqlite
DB_HOST=localhost
DB_USER=your_user
DB_PASSWORD=your_password
DB_NAME=crusade_db
JWT_SECRET=your_secret_key
```

### Build and Run

```
go build -o crusade-automata
./crusade-automata
```

## API Endpoints

|Endpoint|Method|Description|
|---|---|---|
|`/API/register/`|`POST`|User registration (hashes password)|
|`/API/login`|`POST`|Authenticates user and returns JWT|
|`/API/battle`|`POST`|Records a battle|
|`/API/update_unit`|`POST`|Updates a unit's data|
|`/API/get_order`|`GET`|Gets an order of battle|

## Plugin System

- Plugins are stored in `/plugins/`.
- Each plugin defines a `plugin` table with metadata and a `process_data(input_json)` function.

### Example Lua Plugin (Auto XP Grant)

```lua
local json = require("json")

plugin = {
    name = "Auto XP Grant",
    description = "Grants XP to all units after a battle",
    event_trigger = "post_battle"
}

function process_data(input_json)
    local data = json.decode(input_json)
    for _, unit in ipairs(data.units) do
        unit.xp = unit.xp + 1
    end
    return json.encode({ status = "XP updated", battle_id = data.battle_id })
end
```

## Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature-name`)
	- Ideally it is something listed in the project board
3. Commit your changes (`git commit -m 'Added new feature'`)
4. Push to the branch (`git push origin feature-name`)
5. Submit a pull request

## License

Crusade Automata is released under the AGPLv3.0 License.
