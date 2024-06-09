### Introduction

Welcome to the repository of "**3Erefactor**". This repository contains:

- **Datasets** for projects with architectural inconsistencies: including the project's source model (architectureModel/project/project.dep.json), target model (architectureModel/project/project.con.json. For archstudio and hadoop, it's tar.txt), and reflexion model (architectureModel/project/project.rfx.json).
- **"3Erefactor" tool** (3Erefactor-jar-with-dependencies.jar) designed for automated refactoring recommendations aimed at eliminating architectural inconsistencies. 
- **Ground truth** of a list of refactoring operations that address architectural inconsistencies for each project.
- **Standardized output** in Json format of the tool.
-  A **formal proof** (Formal Proof.pdf) confirms that pairs of two operations are sufficient to determine the invalid operation to be removed, which can be used to support Executability Constraint Strategy of 3Erefactor.

### Usage

```bash
$ java -jar 3Erefactor-jar-with-dependencies.jar --help
usage: refactor [--version] [--help] <command> [<args>]
There are common refactor commands used in various situations:
    search    generate refactoring operations list
    feedback  interactively select operations
'refactor --help' list available subcommands and some concept guides.
See 'refactor <command> --help' to read about a specific subcommand or concept.
```

```bash
$ java -jar 3Erefactor-jar-with-dependencies.jar search --help

usage: refactor search [<options>]

    -d, --dependency        specify dependency file path
    -t, --target            specify target file path
    -r, --reflexion         specify reflexion file path
    -h, --DRH               specify architecture recovery file path (optional)
    -o, --output            specify output result file path (optional)
    -i, --iteration         specify max iteration size (optional)
    -p, --populationSize    specify population size (optional)
    -s, --solutionSize      specify generated solutions size (optional)
    -m, --mutation          specify mutation possibility (between 0~1) (optional)
```

```bash
$ java -jar 3Erefactor-jar-with-dependencies.jar feedback --help
usage: refactor feedback [<options>]

    -a, --accept            specify accepted operation list file path (optional)
    -b, --reject            specify rejected operation list file path (optional)
    -s, --search            specify search file path
    -o, --output            specify output result file path (optional)
```

### Examples:

1. Using the provided source model, target model, and reflexion model, the refactoring solutions generated by the search command aim to reduce architectural inconsistencies between code implementation and architectural design.

```bash
java -jar 3Erefactor-jar-with-dependencies.jar search \
    --dependency=architectureModel/depends/depends.dep.json \
    --target=architectureModel/depends/depends.con.json \
    --reflexion=architectureModel/depends/depends.rfx.json \
    --DRH=architectureModel/depends/dependsArchDRH.json # optional; This makes the algorithm converge faster
    --output=out.json   \  # optional; default value = ${PWD}/refactor.search.out.json
    --iteration=10 \  # optional; default value = 10
    --mutation=0.1 \  # optional; default value = 0.1
    --populationSize=10 \  # optional; default value = 10
    --solutionSize=15 \  # optional; default value = 15
```

2. The feedback command allows users to interact with the generated refactoring solutions, choose an optimal solution in Pareto front, and iteratively generate new solutions based on feedback.

```bash
java -jar target/3Erefactor-jar-with-dependencies.jar feedback \
    --accepted=a.json \   # optional
    --rejected=b.json \   # optional
    --search=refactor.search.out.json \
    --output=out.json     # optional; default value = ${PWD}/refactor.feedback.out.json
```