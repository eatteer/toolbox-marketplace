# Before / After Examples

## Python: service class

Before:

```python
class us:
    def __init__(self,db):
        self.db=db
    def get(self,id):
        u=self.db.query("SELECT * FROM users WHERE id=?",id)
        if u==None: return None
        if u['st']==1: u['active']=True
        else: u['active']=False
        return u
```

After:

```python
ACTIVE_STATUS = 1


class UserService:
    def __init__(self, database):
        self.database = database

    def get_by_id(self, user_id):
        user = self.database.query(
            "SELECT * FROM users WHERE id = ?",
            user_id,
        )

        if user is None:
            return None

        user["active"] = user["st"] == ACTIVE_STATUS

        return user
```

## JavaScript / TypeScript: HTTP request

Before:

```javascript
async function f(u){const r=await fetch('/api/users/'+u);if(!r.ok){throw new Error('err')}const d=await r.json();return d.items.filter(x=>x.a).map(x=>({n:x.name,e:x.email}))}
```

After:

```javascript
async function getActiveContacts(userId) {
  const response = await fetch(`/api/users/${userId}`);

  if (!response.ok) {
    throw new Error(`Failed to load user ${userId}: ${response.status}`);
  }

  const { items: contacts } = await response.json();

  return contacts
    .filter((contact) => contact.active)
    .map((contact) => ({
      name: contact.name,
      email: contact.email,
    }));
}
```

## SQL

Before:

```sql
select u.name,count(o.id) from users u left join orders o on o.user_id=u.id where u.active=1 group by u.name having count(o.id)>5 order by 2 desc
```

After:

```sql
SELECT
    u.name,
    COUNT(o.id) AS total_orders
FROM users AS u
LEFT JOIN orders AS o
    ON o.user_id = u.id
WHERE u.active = 1
GROUP BY u.name
HAVING COUNT(o.id) > 5
ORDER BY total_orders DESC;
```

## Named conditions

Before:

```javascript
if (user.role === "admin" || (post.authorId === user.id && !post.locked && Date.now() - post.createdAt < 3600000)) {
  editPost(post);
}
```

After:

```javascript
const ONE_HOUR_MS = 60 * 60 * 1000;

const isAdmin = user.role === "admin";
const isAuthor = post.authorId === user.id;
const isStillEditable = !post.locked && Date.now() - post.createdAt < ONE_HOUR_MS;

if (isAdmin || (isAuthor && isStillEditable)) {
  editPost(post);
}
```
