## Want cloud build trigger to execute only when you change a specific file?

This blog will show how you can control when your cloud build trigger gets executed. 

My previous blog provided a brief introduction of Google's Cloud build service and steps to create a simple pipeline to deploy a bucket on GCP using Cloud build triggers.

Links -: 
- Article - [Google Cloud Build Pipeline](https://nikhilm.hashnode.dev/google-cloud-build-pipeline)
- Github Repo - [Cloudbuild-demo](https://github.com/nikhilmakhijani/Cloudbuild-demo)

### Problem
Did you guys notice that the trigger gets executed every time you change any file in your repository?? 
For instance I just updated README and trigger got executed?? 
![](https://media.giphy.com/media/ntB3bC8XHWvcVKLnp0/giphy.gif)
**WE DON'T WANT THAT !!!!**

I just want my trigger to get executed when I update my terraform code.
![](https://media.giphy.com/media/l4Epatstdz7owwmic/giphy.gif)

### Solution
Cloud build has this amazing feature which provides us this control using **SHOW INCLUDED FILES AND IGNORED FILES** option.

![Screenshot 2022-01-09 at 4.49.47 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641727261880/BczVfVBxR.png)

We just need to make a small change to our trigger configuration and trigger will only get executed when the terraform code is changed. 
My terraform code resides inside deployments folder. So I have made the following change to the trigger.
![Screenshot 2022-01-09 at 4.55.46 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641727601420/Y7h5UBeEA.png)

Now trigger will get executed if and only if any file residing inside deployments folder is changed.

### Conclusion
We discussed how easily we can control when the trigger will get executed. 
Next article will be on how to use substitution variables in Cloud Build.
![](https://media.giphy.com/media/WUmv5AjoLHUCtW8Zx1/giphy.gif)

