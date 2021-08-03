# FiveM MongoDB wrapper
## Description
This resource is a simple MongoDB wrapper for [FiveM](https://fivem.net/). It's running on top of [MongoDB Node Driver](https://mongodb.github.io/node-mongodb-native/).

## Installation

1. Clone this repository to `resources/mongodb` folder.
2. Copy `mongodb/database.cfg` to your server root directory.
3. Add the following lines to your server config:
```
exec "database.cfg"
start mongodb
```
4. Change `mongodb_url` and `mongodb_database` in `database.cfg`.
5. Run `npm install` in `resources/mongodb` directory.

## Usage

Every callback accepts `success<boolean>` as its first argument. If `success` is `false`, second argument contains error message.

# Find One
Example (Lua):
```lua
exports.mongodb:findOne({ collection = "users", query = { _id = id } }, function (success, result)
    if not success then
        print("Error message: "..tostring(result))
        return
    end
    print("User name is "..tostring(result[1].name))
end)
```

#Aggregate
```lua
    -- how to use 
exports.mongodb:aggregate({ 
	collection = "users",
	pipeline = {
		[1] = { ["$match"] = { identifier = "steam:110000109bbb53b" } },
		[2] = { ["$project"] = { playerName =  1  } } ,
		[3] = { ["$limit"] = 1 }
	}
}, function (success,result)
	if not success then
		print("Error message: "..tostring(result))
	else
	  for _, user in pairs(result) do
		for key, value in pairs(user) do
			print(key, " : " , value)
		end
		print("-------")
	  end
	end
end)
```

#Find
```lua
exports.mongodb:find({
    collection = "phone_message",
    query = {
        transmitter = "",
        receiver = ""
    },
    limit = 100,
    sort = {
        time = -1
    }
}, function (success, result)
    if not success then
        return
    end

    print("\n** 100 message lastest")
    for i, document in ipairs(result) do
        for k, v in pairs(document) do
            print("* "..tostring(k).." = \""..tostring(v).."\"")
        end
    end
end)
```



#CreateIndex
```lua
AddEventHandler("onDatabaseConnect", function (databaseName)
	exports.mongodb:createIndex({
		collectionName = "esx_status",
		keys = { ["identifier"] = 1},
		options= { ["unique"] = true , ["name"] = "identifier" }
	})
end)
```


