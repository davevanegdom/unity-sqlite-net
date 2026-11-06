# FORK FROM [UNITY_SQLITE-NET](https://github.com/gilzoide/unity-sqlite-net)
It is recommended to install the source repository instead of this fork!!


## How to install
Either:
- Use the [openupm registry](https://openupm.com/) and install this package using the [openupm-cli](https://github.com/openupm/openupm-cli):
  ```
  openupm add com.davevanegdom.sqlite-net
  ```
- Install using the [Unity Package Manager](https://docs.unity3d.com/Manual/upm-ui-giturl.html) with the following URL:
  ```
  https://github.com/davevanegdom/unity-sqlite-net.git#1.3.3
  ```
- Clone this repository or download a snapshot of it directly inside your project's `Assets` or `Packages` folder.


## Usage example
The following code demonstrates some of SQLite-net's core functionality.
Check out [SQLite-net's Wiki](https://github.com/praeclarum/sqlite-net/wiki) for more complete documentation on how to use the library.
```cs
using SQLite;
using UnityEngine;

// The library contains simple attributes that you can use
// to control the construction of tables, ORM style
public class Player
{
    [PrimaryKey, AutoIncrement]
    public int Id { get; set; }
    public string Name { get; set; }
}

public class TestSQLite : MonoBehaviour
{
    void Start()
    {
        // 1. Create a connection to the database.
        // The special ":memory:" in-memory database and
        // URIs like "file:///somefile" are also supported
        var db = new SQLiteConnection($"{Application.persistentDataPath}/MyDb.db");

        // 2. Once you have defined your entity, you can automatically
        // generate tables in your database by calling CreateTable
        db.CreateTable<Player>();

        // 3. You can insert rows in the database using Insert
        // The Insert call fills Id, which is marked with [AutoIncremented]
        var newPlayer = new Player
        {
            Name = "gilzoide",
        };
        db.Insert(newPlayer);
        Debug.Log($"Player new ID: {newPlayer.Id}");
        // Similar methods exist for Update and Delete.

        // 4.a The most straightforward way to query for data
        // is using the Table method. This can take predicates
        // for constraining via WHERE clauses and/or adding ORDER BY clauses
        var query = db.Table<Player>().Where(p => p.Name.StartsWith("g"));
        foreach (Player player in query)
        {
            Debug.Log($"Found player named {player.Name} with ID {player.Id}");
        }

        // 4.b You can also make queries at a low-level using the Query method
        var players = db.Query<Player>("SELECT * FROM Player WHERE Id = ?", 1);
        foreach (Player player in players)
        {
            Debug.Log($"Player with ID 1 is called {player.Name}");
        }

        // 5. You can perform low-level updates to the database using the Execute
        // method, for example for running PRAGMAs or VACUUM
        db.Execute("VACUUM");
    }
}
```


## License
SQLite-net for Unity first-party code is licensed under the [MIT license](LICENSE.txt).

Third-party code:
- SQLite-net: [MIT license](Runtime/sqlite-net/LICENSE.txt)
- SQLite3 Multiple Ciphers: [MIT license](https://github.com/utelle/SQLite3MultipleCiphers/blob/main/LICENSE)

Database icons from [Solar Icons Set](https://www.figma.com/community/file/1166831539721848736/solar-icons-set), licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
