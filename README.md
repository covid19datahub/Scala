<a href="https://covid19datahub.io"><img src="https://storage.covid19datahub.io/logo.svg" align="right" height="128"/></a>

# Scala Interface to COVID-19 Data Hub

For Scala you can use following singleton object `Fetcher`. It uses components `java.net.URL` from Java Platform SE and `scala.io.Source` from Scala Standard Library.

The same problem as in Python example occurs here, header `User-Agent` needs to be present, see [similar issue in Java](https://stackoverflow.com/questions/13670692/403-forbidden-with-java-but-not-web-browser).

```scala
// file: covid19datahub.scala
object COVID19Datahub {
  // method to fetch data
  def fetch(): (List[Array[String]],Array[String]) = {
    // download
    val url  = new java.net.URL("https://storage.covid19datahub.io/data-1.csv")
    val conn = url.openConnection
    conn.setRequestProperty("User-Agent","Mozilla/5.0")
    
    // parse
    val text  = scala.io.Source.fromInputStream(conn.getInputStream).getLines
    val lines = text.map(line => line.split(",").map(_.trim)).toList
    
    // split header and data
    val header = lines.head
    val data   = lines.slice(1,lines.size)
    
    // return
    (data,header)
  }
}
```

The object can be used in following way

```scala
// file: main.scala
import covid19datahub.COVID19Datahub
object MainObject {
  def main(args: Array[String]): Unit = {
    val (data,header) = COVID19Datahub.fetch
    
    // prints first and last element
    //data(0).foreach(println)
    //data(data.length-1).foreach(println)
  }
}
```
