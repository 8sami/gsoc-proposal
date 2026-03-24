# IM Wrapper Proposal - Samiullah Javed

<table>
    <thead>
        <tr>
            <th width="203">Personal Details</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Name</strong></td>
            <td>Samiullah Javed</td>
        </tr>
        <tr>
            <td><strong>Organisation Details</strong></td>
            <td>College: Islamia Govt. Science College Sukkur<br>Program: Computer Science<br>Grade: 12th<br>Expected Graduation Date: June 2026</td>
        </tr>
        <tr>
            <td><strong>Contact Details</strong></td>
            <td>Phone No: +92 326 3672475 <br>Email ID: samiullahjavedd@gmail.com</td>
        </tr>
        <tr>
            <td><strong>Current Timezone</strong></td>
            <td>Pakistan Standard Time</td>
        </tr>
        <tr>
            <td><strong>Github Profile</strong> </td>
            <td><a href="https://github.com/8sami">https://github.com/8sami</a></td>
        </tr>
        <tr>
            <td><strong>Linkedin Profile</strong></td>
            <td><a href="https://www.linkedin.com/in/samiullahjaved">https://www.linkedin.com/in/samiullahjaved</a></td>
        </tr>
        <tr>
            <td><strong>Resume</strong></td>
            <td><a href="https://samiullahjaved.com/Samiullah_Javed.pdf">https://samiullahjaved.com/Samiullah_Javed.pdf</a></td>
        </tr>
        <tr>
            <td><strong>Portfolio Website</strong></td>
            <td><a href="https://samiullahjaved.com">https://samiullahjaved.com</a></td>
        </tr>
    </tbody>
</table>

#### Project Proposal

* **Title**: Whatsapp bot for CARE
* **Project Overview**:
    IM wrapper is a djano plugin based instant messaging provider meant to provide staff and patients ease of access to medical data through any messaging app, quickly and securely. Additionally, it also aims to add functionality of sending alerts and notifications via the configured messaging providers.

    Although the care web app already provides all the information, but people such as those living in rural/remote areas could greatly benefit from accessing their medical records and appointments through a simple message on whatsapp instead of having to navigate a web app which could prove difficult for people with limited digital exposure.

    The provider approach makes the instant messaging functionality independant of any messaging app, ensuring each one's support can be developed, maintained and tested without intefering with other messaging provider's implementation.

    Developing it as a plugin ensures that we wont have to worry about it interfering with care's backend. As a plugin, it can be removed, added, updated any time without affecting the core backend. This keeps the core backend clean and lean which ensures good maintainability and good developer experience.

    **Specification:**

    1. **Architecture**: Plugin based, using the [django plugin cookiecutter template](https://github.com/ohcnetwork/care-plugin-cookiecutter).
    2. **Authentication**: Two step authentication, first is matching the requestor's phone number with the number associated with a patient or staff member in the database. The second step is asking for DOB to confirm identity. If the requestor fails to provide correct DOB within 3 attempts, the request is blocked for 15 minutes.
    3. **Authorization**: The authorization is managed via the type of account (staff or patient) and roles permissions logic in the care backend. For example a patient can only access their own data, while a staff member can access the data of patients.
    4. **Caching**: Caching using redis to reduce latency and database load.
    5. **Rate Limiting**: Rate limiting and debouncing to prevent spam and abuse.
    6. **Error Handling**: Proper error handling.
    7. **Audit Logging**: Audit logging using the existing care.audit_log package.
    8. **Testing**: Proper tests using playwright and pytest.
    9. **Documentation**: Documentation using sphinx.
  
    **Flow of Program:**
    This flowchart helps illustrates the flow of program of the IM Wrapper (excluding the alert functionality):

    <a href="https://miro.com/app/board/uXjVG1KxNJM=/">
    <img title="Click to open the flowchart in miro" src="media/flowchart.png" alt="flowchart illustrating the flow of program" width="800"/>
    </a>

    **Proof of Concept:**
    To support my claims and get hands on experience, I developed a working prototype of the IM Wrapper as a django plugin using the [django plugin cookiecutter template](https://github.com/ohcnetwork/care-plugin-cookiecutter) with the help of AI.

    My plan was to architect it as an IM provider which could be extended to support any messaging app. Since it's a proof of concept, there are many things that could have been done better and many things are intentionally kept simple, but I believe it is somewhat successful in properly conveying my ideas.
    
    Below is the list of things I focused on implementing for the POC:

    * Implemented the two step auth, in which the first step verifies the requestor's phone number with the number associated with a patient or staff member in the database. The second step asks for DOB to confirm identity. If the requestor fails to provide correct DOB within 3 attempts, all requests from that number are blocked for 15 minutes.
    * Proper state management is implemented so that the 'bot' is fool-proof and somewhat context-aware.
    * Configurable TTL caching is also implemented. 
    * The plugin is using care.audit_log package to log all the events to comply with HIPAA security regulations.
    * Fetches live data instead of place holder dummy data.
    * Data sanitization is also implemented to prevent sending irrelevant sensitive information that could put PII of patients at risk.

    Below are the links to the Github repo and a YouTube video demonstration of the POC:

    * **Github repo**: [https://github.com/8sami/im-wrapper](https://github.com/8sami/im-wrapper)
    * **YouTube video**: [https://www.youtube.com/watch?v=wKRil3z-d5s](https://www.youtube.com/watch?v=wKRil3z-d5s)


    **Additional Information:**
    * During the development of POC, I created [im_wrapper_setup.sh](im_wrapper_setup.sh) script to help automate the setup and running of the development environment.

        The script [im_wrapper_setup.sh](im_wrapper_setup.sh) pulls the latest changes from origin develop, rebuilds containers, loads fixtures, logins as admin and creates a service account, generates service account token, creates a read only role and assigns it to the service account, gets all organizations and assigns the service account to them then updates the service account token and username in `plug_config.py`.
    * I have also put together [plugin_setup.md](plugin_setup.md) to help guide with the setup of the POC plugin.
    * A few thoughts I had during the development of POC:
        * I wonder if the IM Wrapper plugin will also need a frontend implementation (just like [scribe_fe](https://github.com/ohcnetwork/care_scribe_fe)) to support view once links and for providing user with the ability to download PDFs of invoices and medications, as sending these PDFs via whatsapp might not be a good idea.
        * We will surely need a frontend implementation to be able to send notifications and alerts to patients and staff members.
        * Since each encounter (visit) can have different medications and service requests etc each, it would make more sense to prompt user to select an encounter when they message the plugin for, let's say, medications (if a patient has multiple encounters).
        * I was also thinking of implementing a one-time otp verification, just to be extra careful, but since we will be matching the requestor's phone number against the db and the requestor will already have access to the phone number, I think it will just be an additional cost.

    **Use Cases**:
    1. Since care provides teleICU services to many remote areas of India, it makes a lot of sense to provide ease of access to medical data to the people living in those areas where issues like internet connectivity, digial literacy and lack of access to computers are prevalent.
    2. Using messaging apps like whatsapp is more comfortable and easier to use for people because of its familiarity than navigating a web app, which can be daunting for some.
    3. Accessing information via whatsapp is much more convenient and faster than having to log in to the care web app.

        The image below aims to depict the time it may take to access information via both methods by showing the difference in number of steps:

        <img title="Difference in time taken to access medical info" src="media/difference_in_time_taken.png" alt="image illustrating the difference b/w the time it takes to access info" width="800"/>

* **Features**:
  1. List of five features I appreciate in the platform

#### Technical Skills and Relevant Experience

* My technical skills include Python, Javascript, Typescript, React, Nextjs, Django, Flask, SQL, Git, Github, Docker, Linux
* My first full stack project, "Simple Invoice Generator" was built using Django, weasyprint, crispy-bootstrap5 and jinja. That project recorded almost 2 Cr of transactions for a procurement service provider and then as I was developing its v2 using nextjs, shadcn, reactPDF and django ninja the business completed its tenure. I reviewed the code a few weeks ago... it needs a lot of work but I plan to deploy it as a free open source tool this year.

    I have a YOE working as a software dev remotely for an Australian agency where I got the chance to work on production grade code across various projects using different technologies. Working in a high stakes environment has taught me a lot about problem solving while respecting tight deadlines.

#### Implementation Timeline and Milestones

* Week-by-week plan outlining goals, tasks, and deliverables
* Specific milestones for project phases

#### Summary About Me

A short intro of me: [Watch on YouTube](https://youtube.com/shorts/5Gx_Yw9gSZU?si=rSZAJvbkG9n7dxrv) :)

 I am a curious person. I like trying out new stuff and I mostly do things that seem fun to me. Problem solving and product development are one of those things that I very much enjoy doing. I have more than a YOE working as a software developer in an Australian agency where I resigned from in february to explore my interests and focus on my studies to try and get into MIT. I started programming when I was in 9th grade, as it seemed really interesting, and its just as fun now as it was back then.

 My motivation for winning gsoc is that it aligns with my goals, and I have also developed a love for open source in the process. I genuinely enjoy contributing to something bigger than me, something that would go on to live and make people's lives easier even after me.

#### Availability and Commitment

* 40-50 hours per week
* I'll be studying for SAT and training for national powerlifting competition for 2-3 hours daily. Will be done with Board exams by 25 april 2026 and I won't be preparing for entrance exams until next year.

#### Contribution to OHC Repo (if applicable)

* **Pull Requests**:
  1. <https://github.com/ohcnetwork/care_fe/pull/16144>
  2. <https://github.com/ohcnetwork/care_fe/pull/16086>
  3. <https://github.com/ohcnetwork/care_fe/pull/16085>
  4. <https://github.com/ohcnetwork/care_fe/pull/15828>
  5. <https://github.com/ohcnetwork/care_fe/pull/15546>
  6. <https://github.com/ohcnetwork/care_fe/pull/15454>
  7. <https://github.com/ohcnetwork/care_fe/pull/15098>

* **Issues**: 
  1. <https://github.com/ohcnetwork/care_fe/issues/15719>
  2. <https://github.com/ohcnetwork/care_fe/issues/15494>

* I am not sure if I should mention this, but I lowkenuinely really love guiding and helping other people out in the community :D