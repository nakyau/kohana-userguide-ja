# モデル {#models}

Wikipediaより:
<div class="original-doc">
# Models

From Wikipedia:
</div>

 > The model manages the behavior and data of the application domain,
 > responds to requests for information about its state (usually from the view),
 > and responds to instructions to change state (usually from the controller).

シンプルなモデルを作るには:

	class Model_Post extends Model
	{
		public function do_stuff()
		{
			// ここにロジックを書く
		}
	}

<div class="original-doc">
Creating a simple model:

	class Model_Post extends Model
	{
		public function do_stuff()
		{
			// This is where you do domain logic...
		}
	}
</div>

データベースにアクセスしたければ、このようにModel_Databaseクラスを拡張します。

	class Model_Post extends Model_Database
	{
		public function do_stuff()
		{
			// ここにロジックを書く
		}

		public function get_stuff()
		{
			// データベースから値を得る
			return $this->db->query(...);
		}
	}

CRUD/ORMについては[ORM Module](../../guide/orm)を見てください。
<div class="original-doc">
If you want database access, have your model extend the Model_Database class:

	class Model_Post extends Model_Database
	{
		public function do_stuff()
		{
			// This is where you do domain logic...
		}

		public function get_stuff()
		{
			// Get stuff from the database:
			return $this->db->query(...);
		}
	}

If you want CRUD/ORM capabilities, see the [ORM Module](../../guide/orm)
</div>