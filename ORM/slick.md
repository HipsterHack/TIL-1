# Slick 

Slick은 ORM이라기 보다는 Functional-relational mapper(FRM?) 이다.
아무래도 scala 기반이고 함수형 프로그래밍 기반이다보니 immutable collection에 포커스를 맞춘다.
(ORM은 immutable로 주진 않는다)


## 매핑 방법 
* 아래는 매핑 예제다.

```
type Person = (Int,String,Int,Int)
class People(tag: Tag) extends Table[Person](tag, "PERSON") {
  def id = column[Int]("ID", O.PrimaryKey, O.AutoInc)
  def name = column[String]("NAME")
  def age = column[Int]("AGE")
  def addressId = column[Int]("ADDRESS_ID")
  def * = (id,name,age,addressId)
  def address = foreignKey("ADDRESS",addressId,addresses)(_.id)
}
lazy val people = TableQuery[People]

type Address = (Int,String,String)
class Addresses(tag: Tag) extends Table[Address](tag, "ADDRESS") {
  def id = column[Int]("ID", O.PrimaryKey, O.AutoInc)
  def street = column[String]("STREET")
  def city = column[String]("CITY")
  def * = (id,street,city)
}
lazy val addresses = TableQuery[Addresses]
```
## 객체를 구하는 방법

* 아래 2개의 예제는 ORM처럼 작동하는 메소드이다.  

```
val people: Seq[Person] = PeopleFinder.getByIds(Seq(2,99,17,234))
val addresses: Seq[Address] = people.map(_.address)

```

```
val people: Seq[Person] = PeopleFinder.getByIds(Seq(2,99,17,234)).prefetch(_.address)
val addresses: Seq[Address] = people.map(_.address)

```
### 쿼리 지원

아래와 같이 MySQL등에서 사용하는 prepared 변수형? 쿼리를 지원한다,

```
val hql: String = "FROM Person p WHERE p.id in (:ids)"
val q: Query = session.createQuery(hql)
q.setParameterList("ids", Array(2,99,17,234))
```

# 참고 

http://slick.typesafe.com/doc/2.1.0/orm-to-slick.html
