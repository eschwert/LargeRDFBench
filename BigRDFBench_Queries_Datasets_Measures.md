For all of the given dataset and query features, you first need to download the Java [LargeRDFBench\_Util.jar](http://goo.gl/8ql1UA) and include into the java project. The complete source code for this tool can also be checkout from SVN link http://bigrdfbench.googlecode.com/svn/trunk/.

## Dataset Structuredness ##

Duan et al. " Apples and Oranges: A Comparison of RDF Benchmarks and Real RDF Datasets" introduced the notion of structuredness or choerence, which indicates whether the instances in a dataset have only a few or all attributes of their types set. They show that artificial datasets are typically highly structured and “real” datasets are less structured.  For a given dataset,the structuredness value ranges [0,1] with 0 means less structured and 1 means high structured dataset. A federated SPARQL query benchmark should comprise datasets of varying structuredness values. You can calculate the structuredness value for a dataset (hosted as SPARQL endpoint) as follow.

```
String endpointUrl = "http://localhost:8894/sparql";   //endpoint URL
String namedGraph = "http://aksw.org/benchmark";   // named Graph
double coherence = StructurednessCalculator.getStructurednessValue(endpointUrl, namedGraph);
System.out.println("\nOverall Structuredness or Coherence: " + coherence);

```

A sample output for Affymetrix dataset is given below:
```
Total rdf:types: 3

1 : Type: http://rdfs.org/ns/void#Dataset
Coverage : 0.6
Weighted Coverage : 8.442659916291027E-6

2 : Type: http://bio2rdf.org/affymetrix_vocabulary:Probeset
Coverage : 0.5066791705833019
Weighted Coverage : 0.9999824111251744

3 : Type: http://bio2rdf.org/dataset_vocabulary:Endpoint
Coverage : 1.0
Weighted Coverage : 9.146214909315279E-6

Overall Structuredness or Coherence: 0.506684470477653
```

The details about coverage and weighted coverage can be found in Duan et al. paper.
<font color='red'> The complete structuredness and type coverage results for all of the LargeRDFBench datasets is given <a href='http://goo.gl/XpSSPN'>here</a> </font>

## Query Features ##
Please first read LargeRDFBench paper to understand these query features.

Put all of your benchmark queries in a folder (one query per file). Query Features for all of the queries can be printed as follow.

```
String inputDir= "../LargeRDFBench-Utilities/queries/"; // mind the last /
File folder = new File(inputDir);
File[] listOfFiles = folder.listFiles();
long count = 1; 
for (File qryFile : listOfFiles)
  {	
    BufferedReader br = new BufferedReader(new FileReader(inputDir+qryFile.getName()));
    String line;
    String queryStr="";
	while ((line = br.readLine()) != null) 
                {
		   queryStr= queryStr+" "+line;
		}
         br.close();
    System.out.println("--------\n"+count+ ": "+qryFile.getName()+" Query: " + queryStr);
    QueryStatistics.printQueryStats(queryStr,qryFile.getName());
    count++;
   }
```

A sample output is given below
```
25: S2 Query:  SELECT ?party ?page  WHERE {    <http://dbpedia.org/resource/Barack_Obama> <http://dbpedia.org/ontology/party> ?party .    ?x <http://data.nytimes.com/elements/topicPage> ?page .    ?x <http://www.w3.org/2002/07/owl#sameAs> <http://dbpedia.org/resource/Barack_Obama> . }  
Basic Graph Patterns (BGPs): 1
Triple Patterns: 3
Total Vertices:7 ==> [x, page, http://www.w3.org/2002/07/owl#sameAs, http://dbpedia.org/ontology/party, http://data.nytimes.com/elements/topicPage, http://dbpedia.org/resource/Barack_Obama, party]
Join Vertices: 2 ==> [x, http://dbpedia.org/resource/Barack_Obama]
Join Vertices to Total Vertices ratio: 0.2857142857142857
     x Join Vertex Degree: 2, Join Vertex Type: Star
     http://dbpedia.org/resource/Barack_Obama Join Vertex Degree: 2, Join Vertex Type: path
Mean Join Vertices Degree: 2.0
--------------------------------------------------------------
26: S3 Query:  SELECT ?president ?party ?page WHERE {    ?president <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/President> .    ?president <http://dbpedia.org/ontology/nationality> <http://dbpedia.org/resource/United_States> .    ?president <http://dbpedia.org/ontology/party> ?party .    ?x <http://data.nytimes.com/elements/topicPage> ?page .    ?x <http://www.w3.org/2002/07/owl#sameAs> ?president . }  
Basic Graph Patterns (BGPs): 1
Triple Patterns: 5
Total Vertices:11 ==> [http://www.w3.org/2002/07/owl#sameAs, http://dbpedia.org/ontology/nationality, page, http://www.w3.org/1999/02/22-rdf-syntax-ns#type, http://data.nytimes.com/elements/topicPage, x, party, http://dbpedia.org/ontology/party, president, http://dbpedia.org/resource/United_States, http://dbpedia.org/ontology/President]
Join Vertices: 2 ==> [x, president]
Join Vertices to Total Vertices ratio: 0.18181818181818182
     x Join Vertex Degree: 2, Join Vertex Type: Star
     president Join Vertex Degree: 4, Join Vertex Type: Hybrid
Mean Join Vertices Degree: 3.0
--------------------------------------------------------------
```
<font color='red'>
A complete list of query features for all of the LargeRDFBench queries can be found <a href='http://goo.gl/eeW5W0'>here</a></font>

## Triple Patterns Selectivities ##
Please checkout the BigRDF Utilities source code  from SVN link http://bigrdfbench.googlecode.com/svn/trunk/. Set the following required information in the org.aksw.simba.bigrdfbench.util.selectivity class.

```
 static HashMap<String, String> stmtFilters = new  HashMap<String, String>(); // A hashmap contains triple patter as key corresponding filter clause as value for those triple patterns whose selectivity is depended upon Filter clause. load this map from file /queryfilters/stp.txt (strict format is must, should be done via coding)
	 static HashMap<String, String> epToResults = new  HashMap<String, String>(); // endpoint url to total number of results.
	 static List<String> endpoints =  Arrays.asList(
			         "http://localhost:8890/sparql",
				 "http://localhost:8891/sparql",
				 "http://localhost:8892/sparql",
				 "http://localhost:8893/sparql",
				 "http://localhost:8894/sparql",
				 "http://localhost:8895/sparql",
				 "http://localhost:8896/sparql",
				 "http://localhost:8897/sparql",
				 "http://localhost:8898/sparql",
				 "http://localhost:8899/sparql",
				 "http://localhost:8887/sparql",
				 "http://localhost:8888/sparql",
				 "http://localhost:8889/sparql"
			   
			);
	private static RepositoryConnection con = null;
```

Then run the following code.
```
                loadStatementFilters("../LargeRDFBench-Utilities/queryfilters/stp.txt");
		loadEPtoRSfromFile("../LargeRDFBench-Utilities/datasetsizes/sizes.txt");
		String inputDir= "../LargeRDFBench-Utilities/queries/";
		File folder = new File(inputDir);
		File[] listOfFiles = folder.listFiles();
		long count = 1; 
		for (File qryFile : listOfFiles)
		{	
		BufferedReader br = new BufferedReader(new FileReader(inputDir+qryFile.getName()));
		String line;
		String queryStr="";
		while ((line = br.readLine()) != null) {
		   queryStr= queryStr+" "+line;
		}
		br.close();
		
		System.out.println("-----\n"+count+ ": "+qryFile.getName()+" Query: " + queryStr);
		     printQueryStats(queryStr,qryFile.getName());
	   	     count++;
		}
```

A sample output is for query S1 given below

```
11: S1 Query:  SELECT ?predicate ?object WHERE {    { <http://dbpedia.org/resource/Barack_Obama> ?predicate ?object }    UNION        { ?subject <http://www.w3.org/2002/07/owl#sameAs> <http://dbpedia.org/resource/Barack_Obama> .      ?subject ?predicate ?object }  }
Basic Graph Patterns (BGPs): 2

Triple pattern: <http://dbpedia.org/resource/Barack_Obama>  ?predicate ?object
    endpoint:  http://localhost:8891/sparql  (matched results: 77, total results: 42849609, selectivity: 1.796982558230578E-6)
Average (across all datasets) Triple pattern selectivity: 1.796982558230578E-6

Triple pattern: ?subject <http://www.w3.org/2002/07/owl#sameAs>  <http://dbpedia.org/resource/Barack_Obama> 
    endpoint:  http://localhost:8897/sparql  (matched results: 1, total results: 335198, selectivity: 2.983311356273009E-6)
Average (across all datasets) Triple pattern selectivity: 2.983311356273009E-6

 Triple pattern: ?subject ?predicate ?object, 
  for all endpoints   Selectivity: 1.0
Average (across all datasets) Triple pattern selectivity: 1.0

Mean query selectivity (average of of the mean triple pattern selectivities): 0.33333492676463816
Query Selectivities standard deviation: 0.5773488892379413
Triple Patterns: 3
```
<font color='red'>
The mean triple pattern selectivities along with complete details, for all of the LargeRDFBench queries can be found <a href='http://goo.gl/fDNXj9'>here</a> </font>