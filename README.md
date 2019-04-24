### Development-Environment-Help

#### Salesforce Developer Website

[Dev Website](https://developer.salesforce.com/docs/atlas.en-us.salesforce1.meta/salesforce1/dev_learning_more.htm)

#### Git Command Line Crash Course:

See current changes: *git status*

Create a new branch: *git checkout -b my-branch*

Merge branch: *git checkout branch-to-merge-into then git merge my-branch --no-ff*

Add your changes: *git add path/to/your/file or git add .*

Undo adding changes: *git checkout -- path/to/file*

Commit changes (after adding): *git commit -m "your helpful commit message"*

Undo commit (put changes back to staging): *git reset --soft HEAD^*

Push changes to server: *git push origin my-branch*

#### SOURCE of TRUTH (for everything, declarative and programmatic):

[Part 1](https://www.credera.com/blog/technology-solutions/salesforce-introduction-the-situation-the-complication-and-the-solution/)

[Part 2](https://www.credera.com/blog/technology-solutions/developers-guide-to-salesforce-dx-and-source-control/)

[Part 3](https://www.credera.com/blog/technology-solutions/reflections-on-dreamforce-2018-innovations-favorites-einstein/)


##### THE SOLUTION
Enter: the Scratch Org. Salesforce DX gives us the ability to create temporary, personal orgs where changes can be made in isolation, pulled into a local environment, entered into source control, and then deployed to Sandbox and Production orgs. This feature is made available when DevHub is enabled in an Enterprise, Performance, or Unlimited Edition org.

The Scratch Org can be thought of as a private development playground—a place where a developer can write code and customize configurations without the fear that their changes will interfere with anyone else’s work (or vice versa). They are created based on a JSON definition file that can be customized with a large set of configurations and preferences, allowing for a quick setup of an org that approximates the shape of a production org. Scratch Orgs are owned solely by the person who creates them, and they are simple to create and destroy. So simple, in fact, that Salesforce gives them a maximum lifespan of 30 days, a feature that prevents developers from working in a persistently stale environment. And though the Scratch Orgs are owned independently, the definition file can be managed in the repository so that all developers are working in identically shaped environments.

Scratch Orgs bring a whole new dimension to Salesforce development: Not only do they allow for code to be written in an IDE and pushed to a playground for testing, they also open up the possibility to capture the configuration changes made through the Salesforce UI—all of the “click, not code” work—in metadata xml files. This means that all declarative changes can be tracked in source control, something that was nearly impossible until now. While Salesforce has had the idea of a source of truth, most likely the Production org, it keeps no record of when, why, or by whom specific changes were made. These are essential features of source control for developers, so their absence makes it hard to consider any traditional Salesforce environment a true source of truth in the same way that a Git repository is.

With the introduction of Scratch Orgs and the ability to manage changes in a more traditional development environment, the role of Change Sets is heavily diminished. Customizations no longer need to be tracked manually; DX can capture all changes from developers’ Scratch Orgs in source control.

##### THE DRAWBACKS
While the Scratch Org has vastly improved the development life cycle within the Salesforce ecosystem, it is still a relatively new product that has potential for improvement.

One significant hurdle surrounding development in Scratch Orgs is the inability to create one with configurations that exactly mirror the DevHub it is created with. There are many settings that can be defined in the previously mentioned Scratch Org definition file, but it is tedious to figure out what settings you need. Many of the options are poorly documented, and some do not even work. Furthermore, there are some settings the definition file does not account for, so you are left with something that is close but not quite the same. These differences between the development and destination environments can cause unexpected issues when changes are moved from a Scratch Org to a Sandbox. They also make it difficult, if not impossible, to manage profiles with DX, which creates a lot of headaches and can leave behind a long list of manual updates to make after changes are deployed to a new org. As a solution for this, Salesforce has introduced the idea of Shapes, which would provide the ability to mirror Production in a Scratch Org. However, at the time of this writing, this is a feature that is still in pilot and not recommended for general use.

Another limitation presents itself when considering Salesforce’s recommendation to break an org into “artifacts,” and the fact that DX does not come with any sort of merge capabilities. The recommendation around artifacts is that the functionality and metadata in your org should be broken up into smaller pieces that can be managed independently. Salesforce also recommends that things like custom objects and fields should only be used in one artifact each—no sharing. However, this limitation is not realistic; when working with a highly customized org, it is likely that completely different workstreams will rely on the same metadata every once in a while. And if you do manage an object or a field in two different artifacts, whichever changes are most recent will always win, leaving you at risk of overwriting someone else’s work. Thankfully, Salesforce has also introduced a solution to this in second-generation packaging, a feature that, although still in Beta, has proven to be helpful in creating artifacts that contain shared metadata and can be listed as a dependency for other bodies of work. There is still room for improvement, but these new features prove that Salesforce is working hard to get this right; early adopters simply have to put up with the growing pains.

##### 1. Snapshots
One of the major pain points of using DX has been the configuration of the Scratch Org. While the tool is a vast improvement to previous solutions, there is no way to automate certain org settings that may be needed for more complex applications. This results in a developer having to manually configure the Scratch Org after creation depending on the needs of the environment. With the introduction of Scratch Org Snapshots, developers will be able to create a template for their Scratch Orgs so this manual configuration will no longer be necessary. This will be a huge timesaver and will open up the door for more automation possibilities in complex scenarios.

It is important to note that Snapshots will still require a small amount of manual setup. Users will have to configure the first Scratch Org, which will be used to generate the template.

##### 2. Packaging and Dependency Management
While this has been possible since the release of DX, packaging will see some improvements in the near future. This isn’t necessarily due to an upcoming feature, but rather through the conception of a Salesforce development framework known as Force-DI and a corresponding set of libraries. The work done by Andy Fawcett, an active Salesforce contributor and vice president of product management at Salesforce, and his colleagues will allow Salesforce developers to introduce dependency injection to their development lifecycle, something that has been sorely-needed. With the introduction of this framework, we will begin to see decoupled Salesforce services, modularized organizations, and mocking for unit tests.

##### 3. Enterprise Message Platform (EMP)
EMP is not directly related to DX, but it is still a feature that is bringing Salesforce up to speed in the development world. EMP introduces an event-driven architecture to Lightning (Salesforce’s React-esque front-end framework). With an EMP component, a developer is able to generate a platform event, which can be received by multiple listeners to take further action. This will result in simplified code for more complex action. More information on EMP events can be found [here](https://developer.salesforce.com/docs/atlas.en-us.platform_events.meta/platform_events/platform_events_intro_emp.htm).
