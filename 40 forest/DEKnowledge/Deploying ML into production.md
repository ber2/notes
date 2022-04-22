Some notes on deploying [[ML]] into [[Production]] after reading the following two posts:
- [[Hello, production]]
- [[Scikit-learn, meet Production]]

### Notes

A very useful post with ideas on how to quickly deploy [[ML]] projects into production by serving an API using [[fastAPI]] and [[minikube]].

This follows a philosophy which is very interesting.

> Deploying something useless into production, as soon as you can, is the right way to start a new project. It pulls unknown risk forward, opens up parallel streams of work, and establishes good habits.

Which, when applied to the case of a [[ML]] model, turns into:

> Train the simplest model conceivable and deploy it into production, as soon as you can.

For which, of course, the [[Dummy Model]]s available in [[scikit-learn]]'s `sklearn.dummy` module are great.
