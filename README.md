JPMML-Spark
===========

PMML evaluator library for the Apache Spark cluster computing system (http://spark.apache.org/).

# Features #

* Full support for PMML specification versions 3.0 through 4.2. The evaluation is handled by the [JPMML-Evaluator] (https://github.com/jpmml/jpmml-evaluator) library.

# Prerequisites #

* Apache Spark version 1.5.X or 1.6.X.

# Installation #

Enter the project root directory and build using [Apache Maven] (http://maven.apache.org/):
```
mvn clean install
```

The build produces two JAR files:
* `pmml-spark/target/pmml-spark-1.0-SNAPSHOT.jar` - Library JAR file.
* `pmml-spark-example/target/example-1.0-SNAPSHOT.jar` - Example application JAR file.

# Usage #

## Library ##

Creating an evaluator instance based on a PMML document in local filesystem:
```java
File pmmlFile = ...;

Evaluator evaluator = EvaluatorUtil.createEvaluator(pmmlFile);
```

Scoring Apache Spark RDDs using function class `org.jpmml.spark.PMMLFunction`:
```java
Function<Row, Row> pmmlFunction = new PMMLFunction(evaluator);

JavaRDD<Row> input = ...;
JavaRDD<Row> output = input.map(pmmlFunction);
```

Scoring Apache Spark SQL DataFrames using transformer class `org.jpmml.spark.PMMLPredictionModel`:
```java
Transformer pmmlTransformer = new PMMLPredictionModel(evaluator);

DataFrame input = ...;
DataFrame output = pmmlTransformer.transform(input);
```

**A note about building and packaging JPMML-Spark applications**. The JPMML-Evaluator library depends on JPMML-Model and Google Guava library versions that are in conflict with the ones that are bundled with Apache Spark and/or Apache Hadoop. This conflict can be easily solved by relocating JPMML-Evaluator library dependencies to a different namespace using the [Apache Maven Shade Plugin] (https://maven.apache.org/plugins/maven-shade-plugin/). Please see the JPMML-Spark example application for a worked out example.

## Example application ##

The example application JAR file contains an executable class `org.jpmml.spark.EvaluationExample`.

This class expects three command-line arguments:

1. The path of the **model** PMML file in local filesystem.
2. The path of the **input** CSV file in local filesystem.
3. The path of the **output** directory in local filesystem.

For example:
```
spark-submit --master local --class org.jpmml.spark.EvaluationExample example-1.0-SNAPSHOT.jar DecisionTreeIris.pmml Iris.csv /tmp/DecisionTreeIris
```

# License #

JPMML-Spark is licensed under the [GNU Affero General Public License (AGPL) version 3.0] (http://www.gnu.org/licenses/agpl-3.0.html). Other licenses are available on request.

# Additional information #

Please contact [info@openscoring.io] (mailto:info@openscoring.io)
