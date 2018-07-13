\# Array[Row] to DataFrame

\#spark

```scala
import sqlContext.implicits._
val f = e.map({
  case org.apache.spark.sql.Row(a:String,b:String,c:String,d:String,e:String,f:Int) => (a,b,c,d,e,f)
}).toDF()
```