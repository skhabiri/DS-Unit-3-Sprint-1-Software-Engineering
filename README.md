# DS-Unit-3-Sprint-1-Software-Engineering
Software Engineering and Reproducible Research for Data Science

## The World Beyond Notebooks

Python Notebooks are great - they let us explore data and communicate and share
results. But if you want to write more general-purpose reusable code, you should
put it in a *package* - like numpy, pandas, and the other great tools we depend
on.

A full production-grade library is a large undertaking, but this week we will
build our own modest but still useful package with utility functions for common
data science tasks. Behold, **skestimate**!
_________

**Package_Virtual Environment-311:** 


**skestimate: https://pypi.org/project/skestimate/**


# How to publish the package:


A repository named EstimatorPkg is created on github and cloned locally. We put the meta data in the top level of this repo and create a package named skestimate as a subdirectory. This package is going to be published on test pypi. During development importing the package is done in the virtual environment to ensure consistency.


`(EstimatorPkg) EstimatorPkg%> python
>>> import skestimate        # executes __init__.py inside skestimate. Import also creates a sub namespace for skestimate which can be viewed by dir(skestimate). We can have sub packages in the package by creating a subdirectory containing ann__init__.py file. 


### Here are the steps to follow:


1. Create the repo EstimatorPkg in the github with readme, git ignore for python and MIT license
2. Clone  the repo onto the local machine `repos%> git clone https://github.com/skhabiri/EstimatorPkg.git`
3. Use `pyenv install 3.7` to install a specific python version for the package
4. Create a virtual env for the project EstimatePkg. `EstimatorPkg %> pipenv --python 3.7`
   1. Now a virtual environment including a Pipfile has been created. Pipfile can be edited manually too. We install all the required packages inside an activated virtualenv to update the environment with all the requirements through Pipfile, and Pipfile.lock.
5. Activate the virtual env. `EstimatorPkg%> pipenv shell`
   1.  `(EstimatorPkg) EstimatorPkg%> which python`        # points to the location of a specified version of python used in virtual env
6. Install the required packages in the virtual env. `(EstimatorPkg) EstimatorPkg%> pipenv install numpy pandas scikit-learn matplotlib`
7. Create the package directory and __init__.py file. `(EstimatorPkg) skestimate%> touch __init__.py `
8. Code the python modules or subpackages as needed. (fit_est.py)
9. To test the package locally:
   1. `EstimatorPkg) EstimatorPkg%> python`        # launch CLI python repl
   2. `import skestimate`        # import the package
   3. `est = skestimate.example()`        #load cover_type dataset and create an instance of Xest
   4. est.xscore(fit=True)        # test any method of the class
10.  Add twine to the dev packages for uploading the package onto the pypi `(EstimatorPkg) skestimate%> pipenv install -d twine`
11. Create a setup.py and build the actual package `python setup.py sdist bdist_wheel`
   1. The instruction for pypi is in setup.py as a meta file located in the project directory (EstimatePkg). setup.py uses setuptools.setup() to define the package. 
   2. Note that package names in pypi require an underscore to be changed to dashe as a pypi convention. 
12. `(EstimatorPkg) EstimatorPkg%> python setup.py sdist bdist_wheel`        # builds the package based on the setup file. The binary is specific to an OS. Now we should have dist and build directories and skestimate.egg-info, next to the package directory.
13. If not yet, register for a [test pypi account](https://test.pypi.org/account/register/)
14. Upload the package to the test pypi using `twine upload --repository-url https://test.pypi.org/legacy/ dist/*`
   1. #upload the content of the dist/ into test.pypi.org
   2. Or `twine upload dist/*` for uploading onto pypi.org
15. Start a Colab notebook, and install the package `!pip install --index-url https://test.pypi.org/simple/ your_package`
16. Import the package in the notebook, and try it out!




### skestimate package


This package is used to fit and get various metrics for a multi class classifier. It is based on scikit-learn library and uses the methods from that library.


fit_est module inside the package has a class named Xest(self, estimator, data, target_label, ts).
* estimator: A piplined classifier that encodes all the categorical features.
* data: Raw dataset including the target label
* target_label: A string type representing the name of the target label
* ts: A number between 0 and 1 that specifies the test portion of the data.


Below is an example of how to use the class methods on the CoverType data set from UCI repository.
```
$ data = pd.read_csv("https://github.com/skhabiri/PredictiveModeling-CoverType-u2build/blob/master/data/train.csv?raw=true")
rfc = make_pipeline(
    RandomForestClassifier(bootstrap=True, ccp_alpha=0.0, class_weight=None,
                           criterion='entropy', max_depth=14, max_features=20,
                           max_leaf_nodes=None, max_samples=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=2, min_samples_split=10,
                           min_weight_fraction_leaf=0.0, n_estimators=10, n_jobs=-1,
                           oob_score=False, random_state=42, verbose=0,
                           warm_start=False)
                    )
xest = skestimate.Xest(rfc, data, "Cover_Type", 0.2)
```


For local testing we can use the example() function in fit_est.py.
```
>>> import skestimate        
>>> myest = skestimate.example()
>>> myest.xskew(0.9)
```


**Available methods associated with Xest class:**
* xunique():
Reports counts of unique values in each column of data


* xskew(imb=0.99):   
Returns a pandas Series of the sorted column with skewness more than imb. imb is between 0 and 1


* xfit():    
Fits the pipeline estimator and returns fitted estimator, training score, and test score


* xscore(fit=True):   
Calculates accuracy, recall and precision of a classifier and plots the confusion matrix


------------------------------------------------------------------------


**OOP-PEP8-312:** 


An example of Animal class in https://github.com/skhabiri/lambdata repo. The python filename is oop_examp.py. It defines a class of Animal and includes the __init__ method to construct the class by receiving the parameters and storing them as an instance field, such as self.name. Other methods in the class gain access to the instance field by passing self as a parameter to them. In calling class_instance.method() we implicitly pass the self to the method and do not need to specify it inside parentheses. Tiger is a subclass of Animal. It overwrites Animal __init__ method as it needs to add more parameters (stripes) to the class. By calling super().__init__(params), inside Tiger__init__(), self of the Tiger passes to __init__ of super() as a higher scope variable. An example of instantiation:
`too = Tiger(name="tigi", weight=2000, stripes=100)`
`>>> too.food
'meat'`
self.name and self.weight are updated through super().__init__(). self.food takes its value as default in super(). self.stripes is updated byTarget.__init__ method.
Another example of oop about football is in https://github.com/skhabiri/DS-OOP-Review . 


1. game.py 
   1. Class: Game(teams=[team1, team2], score={team1:score1, team2:score2}, locatio, week).
      1. Method: touchdown(self, team, extra_point=1)        # updates the team score
      2. Method: field_goal(self, team)        # updates the team score
      3. Method: safety(self, team)                # updates the team score
      4. Method: get_winning_team(self)        # returns the name of the higher score and lower score teams among the two
2. players.py
   1. Class: Player(name=None, yards=120, touchdowns=5, safety=1, interceptions=0, field_goals=3)
      1. Method: get_points()                # calculate the total points scored from touchdowns and safety
   2. Class: Quarterback(name=None, yards=130, touchdowns=5, completed_passes=20, interceptions=4, safety=None, field_goals=None)
      1. passing_score()                # calculates score based on intercept and completed_passes
   3. Class: Defensive(name=None, yards=130, touchdowns=5, interceptions=4, safety=None, field_goals=None, tackles=2, sacks=3)
3. season.py
   1. Function: generate_rand_games(n=20)        # based on the team names in possible_values.py generate n random instances of Game(), pick pair of random teams and assign random scores. Returns list of games
   2. Function: season_report(games): saves list of winning and losing teams for each game, and saves the total scores of all winning and all losing teams. Saves a dictionary of each team with it’s total scores. Record number of wins and losses for each team in a dictionary. Report each team’s number of wins and lossed, report the average score of a winning/losing team. Report the team with overall highest score as the winner.
------------------------------------------------------------------------
**Containers and Reproducible Builds-313:** 


Install Docker Desktop and create login in docker.com. Container can create a minimally required operating system that is more efficient in doing specialized work. Docker can also save on the hardware expenditure as it can create different environment on docker daemon and stream it to the docker client on our local machine. We can use our local machine and `docker` command to run a client docker or use a web based docker client as follows.
1. Log into https://labs.play-with-docker.com/
2. Add a new Instance
3. `apk add nano` adds nano text editor to the container. This is specific to the host OS distribution which is arch linux in this case, and it’s not related to the docker.
4. `which docker` verifies existence of docker.
5. `docker run hello-world`        # since it’s not recognised locally docker client contact docker daemon to pull the hello-world image from docker hub and create a docker container based on that image which runs an executable “hello from docker world” and stream it back to the docker client to print it on our local terminal.. 
6. `docker run -it debian /bin/bash`        # create a container based on debian distribution of linux with bash shell interactive mode.
7. `nano`                # doesn’t work anymore as we are in a new container. Now we are in debian linux not arch linux anymore.
8. `exit` we exit from debian and return to the jost arch linux OS that we were before
9. `docker run -it debian /bin/bash`        # will create a new container with a new hash numbers. The idea is containers are disposable and we can recreate them from images.
10. `apt update`                # update all the packages
11. `apt install python3`        # install python3 on the Debian distribution of Linux kernel.
12. `python3`                # we can run python3 on Debian Linux!
13. `apt-get install python3-pip`        # to get pip3
14. `pip3 install skestimate`        # we can test our package on Debian. Nice!
15. `docker image ls`        # shows all the local images available
16. `docker container ls -a`        # shows container ID and images available locally of the past containers that were recently run. Docker containers only save the differences vs original images and are usually small, but docker images by themselves could be large.
17.  `nano Dockerfile`        # allows us to create a tweaked image based on an existing docker hub image to be able to build a fresh container based on that image every time we launch docker run.
18.  `docker build . -t skestimate`                # build an image based on the Dockerfile in . and tag it with a specific name skestimate
19.  `docker image ls`        # in addition to debian and hello-world, lists skestimate as a local image available to build a container based on.
20.  `docker run -it skestimate`        #create a container based on an image which was originally built according to Dockerfile Dockerfile is a metafile located in the project (repository) directory not package directory. 
21. `docker image rm hello-world`        # We can remove unnecessary images to free up space. However, if there is a container dependent on that image, first we need to remove the container.
22. `docker container rm 91a3932b5cdc`, `docker image rm hello-world`, `docker image ls`
23. `docker run lambdata_skhab python3 -c "import skestimate; print(skestimate.example().xskew(0.9))"`        # without running the image interactively, it runs a command inside a freshly created container based on the image skestimate and exit.
24. Docker can be used for sharing software, and it can be published on hub.docker.com


Our goal is to test our sketimate package in a debian OS through docker image.
Here are the steps:
* Create the Dockerfile in EstimatePkg directory, which is the repository (project) directory
   * The specific instructions in Dockerfile are:
      * Based in”debian” image in dockerhub
      * bash shell
      * Python3 and pip3 installed
      * numpy, pandas, scikit-learn,  and matplotlib all installed
      * skestimate package installed
* Build the image on local machine with `docker build . -t skestimate_di`
* Very the existence of the new package with `docker image ls`
* Create and enter a fresh container with `docker run -it skestimate_di’
* Test the package with:
   * python3
   * import skestimate
   * est = skestimate.example()
   * est.xscore(fit=True)
   * exit()        #exit the python repl
* Exit the container with `exit`
------------------------------------------------------------------------
**software-testing-documentation-and-licensing-314**:


unittest supports these concepts: 
* A *test fixture* represents the preparation needed to perform one or more tests, and any associated cleanup actions.
* A *test case* is the individual unit of testing. unittest provides a base class, TestCase, which may be used to create new test cases.
* A *test suite* is a collection of test cases
* A *test runner* is a component that executes tests and provides the output.


*Basic structure of unit test:*
```
import unittest
class mytestcase(unittest.TestCase):
        def test_upper(self):
                        self.assertEqual('foo'.upper(), 'FOO')


def test_split(self):
                       s = 'hello world'
                        self.assertEqual(s.split(), ['hello', 'world'])
                        # check that s.split fails when the separator is not a string
                        with self.assertRaises(TypeError):
                            s.split(2)


if __name__ == '__main__':
    unittest.main()
```
A testcase is created by subclassing unittest.TestCase. The individual tests are defined with methods whose names start with the letters test. This naming convention informs the test runner about which methods represent tests. `unittest.main()` provides a command-line interface to the test script. It will run all the test methods in all the testcases, `python skestimate/test_skestimate.py --verbose`. The verbosity output tests docstring as well. To avoid repetitive test fixtures, we may use the setUp(self) method in a testcase class, which will run before every test method in that class.
unittest.TestCase() assert methods:
* assertEqual(a, b)                #a == b
* assertNotEqual(a, b)                #a != b
* assertTrue(x)                        #bool(x) is True
* assertFalse(x)
* assertIs(a, b)                        #a is b
* assertIsNot(a, b)
* assertIsNone(x)                #x is None
* assertIsNotNone(x)
* assertIn(a, b)                        #a in b
* assertNotIn(a, b)
* with self.assertRaises(SomeException):
                    do_something()
One test method may have multiple assert statements. If any of the assert statements fail, the whole test method fails.


