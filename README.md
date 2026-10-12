# Automate-calendar-with-AI-using-n8n
<h3>In this project, I will demonstrate how to use n8n and ChatGPT to automatically create Google Calendar events from simple text messages. I'm doing this project to learn AI automation and build practical workflows.</h3>

## Sign up for n8n

<h3>What is n8n?</h3>
<p>n8n is a workflow automation tool that connects different apps to automate tasks without needing to write code.</p>


- Head to the <a href='https://n8n.io/'>www.n8n.com</a>
- Select Get **started for free**.
- Enter **your email** address into the **Company Email** section.
- Check your email for a verification link and verify your account.
- Enter your details to start a free trial.
- Click on **Start free 14-day trial**.
- Once you've created your signup details, you'll need to answer a few questions.
- Once your account is all set up, select **Start automating**.


## Start a new workflow

<h3>What is workflow?</h3>
<p>A workflow is a series of steps that work together to complete a task automatically. An AI workflow uses artificial intelligence to automate tasks or make decisions, such as creating Google Calendar events from text messages.</p>


- Select **Build workflow**
- Select **Add first step**
- Here, you're asked for your workflow's **trigger**. So, select **On chat message**

<h3>What is Workflow trigger?</h3>
<p>A workflow trigger is the starting point that tells a workflow when to begin. It can start at a specific time or when an event happens, such as receiving an email or sending a chat message.</p>

## Set up your Chat Trigger

- You're now in your chat trigger's setup!
- We won't be adjusting these settings for now, so select **Close X** on this modal.
- Test your Chat Trigger, At the bottom of the canvas, select **Open Chat**.
- Send your first chat message: **Hello**
- The chat responds with raw JSON data instead of a real response. That's okay! We haven't set up anything in the workflow to process a chat message yet.

## Integrate AI into Your Workflow

<p>Now that our workflow has a chat to receive instructions, we'll add the "brain" of our workflow: an AI model.</p>

- Select the **+** button to add a new node.


