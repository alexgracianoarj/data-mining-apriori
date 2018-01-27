## Data::Mining::Apriori

Perl extension for implement the apriori algorithm of data mining.

### SYNOPSIS

```perl
use strict;
use warnings;
use Data::Mining::Apriori;

# TRANSACTION 103:CEREAL 101:MILK 102:BREAD
#        1101          1        1         0
#        1102          1        0         1
#        1103          1        1         1
#        1104          1        1         1
#        1105          0        1         1
#        1106          1        1         1
#        1107          1        1         1
#        1108          1        0         1
#        1109          1        1         1
#        1110          1        1         1

my $apriori = new Data::Mining::Apriori;

$apriori->{metrics}{minSupport}=0.0155; # The minimum support (required), default value is 0.01 (1%)

$apriori->{metrics}{minConfidence}=0.0155; # The minimum confidence (required), default value is 0.10 (10%)

$apriori->{metrics}{minLift}=1; # The minimum lift (optional)

$apriori->{metrics}{minLeverage}=0; # The minimum leverage (optional)

$apriori->{metrics}{minConviction}=0; # The minimum conviction (optional)

$apriori->{metrics}{minCoverage}=0; # The minimum coverage (optional)

$apriori->{metrics}{minCorrelation}=0; # The minimum correlation (optional)

$apriori->{metrics}{minCosine}=0; # The minimum cosine (optional)

$apriori->{metrics}{minLaplace}=0; # The minimum laplace (optional)

$apriori->{metrics}{minJaccard}=0; # The minimum jaccard (optional)

$apriori->{precision}=2; # Sets the floating point precision of the metrics (required), default value is 3

$apriori->{output}=1;
# The output type (optional): 1 - Export to text file delimited by TAB; 2 - Export to excel file with chart.

$apriori->{pathOutputFiles}='data/'; # The path to output files (optional)

$apriori->{messages}=1; # A value boolean to display the messages (optional)

$apriori->{keyItemsDescription}{'101'}='MILK'; # Hash table reference to add items by key and description
$apriori->{keyItemsDescription}{102}='BREAD';
$apriori->{keyItemsDescription}{'103'}='CEREAL';

my@items=(103,101);
$apriori->insert_key_items_transaction(\@items); # Insert key items by transaction
$apriori->insert_key_items_transaction([103,102]);
$apriori->insert_key_items_transaction([103,101,102]);
$apriori->insert_key_items_transaction([103,101,102]);
$apriori->insert_key_items_transaction([101,102]);
$apriori->insert_key_items_transaction([103,101,102]);
$apriori->insert_key_items_transaction([103,101,102]);
$apriori->insert_key_items_transaction([103,102]);
$apriori->insert_key_items_transaction([103,101,102]);
$apriori->insert_key_items_transaction([103,101,102]);

# or from a data file

$apriori->input_data_file("datafile.txt",",");
# Insert key items by line (transaction), accepts the arguments of path to data file and item separator
```
File contents (example)
```text
103,101
103,102
103,101,102
103,101,102
101,102
103,101,102
103,101,102
103,102
103,101,102
103,101,102
```
```perl
print "\n${\$apriori->quantity_possible_rules}"; # Show the quantity of possible rules

$apriori->{limitRules}=10; # The limit of rules (optional)

$apriori->{limitSubsets}=12; # The limit of subsets (optional)

$apriori->generate_rules;
# Generate association rules to no longer meet the minimum support, confidence, lift, leverage, conviction, coverage, correlation, cosine, laplace, jaccard or limit of rules

print "\n@{$apriori->{frequentItemset}}\n"; # Show frequent items
```
Output messages
```text
12
3 items, 12 possible rules
Large itemset of length 2, 3 items
Processing ...
Frequent itemset: { 102, 103, 101 }, 3 items
Exporting to file data/output_large_itemset_length_2.txt ...
Large itemset of length 3, 3 items
Processing ...
Frequent itemset: { 101, 102, 103 }, 3 items
Exporting to file data/output_large_itemset_length_3.txt ...
101, 102, 103
```
Output file "output_itemset_length_2.txt"
```text
Rules	Support	Confidence	Lift	Leverage	Conviction	Coverage	Correlation	Cosine	Laplace	Jaccard
R1	0,80	0,89	1,11	0,08	1,80	0,90	0,67	0,94	0,62	0,89
R2	0,70	0,78	1,11	0,07	1,35	0,90	0,51	0,88	0,59	0,78
R3	0,80	0,89	1,11	0,08	1,80	0,90	0,67	0,94	0,62	0,89
R4	0,70	0,78	1,11	0,07	1,35	0,90	0,51	0,88	0,59	0,78
R5	0,70	0,87	1,25	0,14	2,40	0,80	0,76	0,94	0,61	0,87
R6	0,70	0,87	1,25	0,14	2,40	0,80	0,76	0,94	0,61	0,87

Rule R1: { 102 } => { 103 }
Support: 0,80
Confidence: 0,89
Lift: 1,11
Leverage: 0,08
Conviction: 1,80
Coverage: 0,90
Correlation: 0,67
Cosine: 0,94
Laplace: 0,62
Jaccard: 0,89
Items:
102 BREAD
103 CEREAL

...
```
Output file "output_itemset_length_3.txt"
```text
Rules	Support	Confidence	Lift	Leverage	Conviction	Coverage	Correlation	Cosine	Laplace	Jaccard
R7	0,60	0,67	1,11	0,06	1,20	0,90	0,41	0,82	0,55	0,67
R8	0,60	0,75	1,25	0,12	1,60	0,80	0,61	0,87	0,57	0,75
R9	0,60	0,86	1,43	0,18	2,80	0,70	0,80	0,93	0,59	0,86
R10	0,60	0,67	1,11	0,06	1,20	0,90	0,41	0,82	0,55	0,67
R11	0,60	0,86	1,43	0,18	2,80	0,70	0,80	0,93	0,59	0,86
R12	0,60	0,75	1,25	0,12	1,60	0,80	0,61	0,87	0,57	0,75

Rule R7: { 102 } => { 101, 103 }
Support: 0,60
Confidence: 0,67
Lift: 1,11
Leverage: 0,06
Conviction: 1,20
Coverage: 0,90
Correlation: 0,41
Cosine: 0,82
Laplace: 0,55
Jaccard: 0,67
Items:
102 BREAD
101 MILK
103 CEREAL

Rule R8: { 102, 103 } => { 101 }
Support: 0,60
Confidence: 0,75
Lift: 1,25
Leverage: 0,12
Conviction: 1,60
Coverage: 0,80
Correlation: 0,61
Cosine: 0,87
Laplace: 0,57
Jaccard: 0,75
Items:
102 BREAD
103 CEREAL
101 MILK

...
```

### DESCRIPTION

This module implements the apriori algorithm of data mining.

### ATTRIBUTES

* `totalTransactions` - The total number of transactions.

* `metrics` - The type of metrics:

	* `minSupport` - The minimum support (required), default value is 0.01 (1%)

	* `minConfidence` - The minimum confidence (required), default value is 0.10 (10%)

	* `minLift` - The minimum lift (optional)

	* `minLeverage` - The minimum leverage (optional)

	* `minConviction` - The minimum conviction (optional)

	* `minCoverage` - The minimum coverage (optional)

	* `minCorrelation` - The minimum correlation (optional)

	* `minCosine` - The minimum cosine (optional)

	* `minLaplace` - The minimum laplace (optional)

	* `minJaccard` - The minimum jaccard (optional)

* `precision` - Sets the floating point precision of the metrics (required), default value is 3

* `limitRules` - The limit of rules (optional)

* `limitSubsets` - The limit of subsets (optional)

* `output` - The output type (optional):

	* 1 - Text file delimited by TAB;

	* 2 - Excel file with chart.

* `pathOutputFiles` - The path to output files (optional)

* `messages` - A value boolean to display the messages (optional)

* `keyItemsDescription` - Hash table reference to add item by key and description.

* `keyItemsTransactions` - Hash table reference to add items by keys and transactions.

* `frequentItemset` - Frequent itemset.

* `associationRules` - A data structure to store the name of the rule, key items, implication, support, confidence, lift, leverage, conviction, coverage, correlation, cosine, laplace and jaccard.

```perl
$self->{associationRules} = {
				'1' => {
					   'confidence' => '0.89',
					   'cosine' => '0.94',
					   'implication' => '{ 102 } => { 103 }',
					   'coverage' => '0.90',
					   'laplace' => '0.62',
					   'jaccard' => '0.89',
					   'support' => '0.80',
					   'correlation' => '0.67',
					   'items' => [
							'102',
							'103'
							],
					   'conviction' => '1.80',
					   'lift' => '1.11',
					   'leverage' => '0.08'
					 },
				#...
```

### METHODS

* `new` - Creates a new instance of Data::Mining::Apriori.

* `insert_key_items_transaction(\@items)` - Insert key items per transaction. Accepts the following arguments:

	* An array reference to key items.

* `input_data_file("datafile.txt",",")` - Insert items per line (transaction). Accepts the following arguments:

	* Data file;

	* Item separator.

File contents (example):
```text
103,101
103,102
103,101,102
103,101,102
101,102
103,101,102
103,101,102
103,102
103,101,102
103,101,102
```

* `quantity_possible_rules` - Returns the quantity of possible rules.

* `generate_rules` - Generate association rules until no set of items meets the minimum support, confidence, lift, leverage, conviction, coverage, correlation, cosine, laplace, jaccard or limit of rules.

* `association_rules` - Generate association rules by length of large itemsets.

### AUTHOR

Alex Graciano, [agraciano@cpan.org](mailto:agraciano@cpan.org)

### COPYRIGHT AND LICENSE

Copyright (C) 2015-2018 by Alex Graciano

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself, either Perl version 5.12.4 or,
at your option, any later version of Perl 5 you may have available.
