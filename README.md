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

<h3>What is a node?</h3>
<p>A node is a building block of a workflow that performs a specific task, such as starting the workflow, processing data, or connecting to another app.</p>

- From the list of options, select **AI**.
- Select **AI Agent**.

## Configure the AI Agent node

<p>The AI agent setup page is split into three parts:</p>


1. The left hand side represents the inputs i.e. the chat messages you send that trigger your workflow.
2. The middle section represents the AI agent's setup.
3. The right hand side represents the outputs i.e. the actions or results your agent will deliver.

- In the middle section, check that:
  1. The **Source for Prompt (User Message)** is set to **Connected Chat Trigger Node**
  2. The **Prompt (User Message)** shows ${{ $json.chatInput }}.
- Select the **X** to close the panel.
- You'll see an AI Agent node on your canvas now, with three different sections. An AI workflow can be broken into three key components:
  1. **Chat Model**, is a type of artificial intelligence (AI) that can understand and respond to text.
  2. **Memory**, which stores information from past interactions.
  3. **Tools**, is a piece of code or third party software that the agent can use to do a task.

## Set up your Chat Model
- Select the **+** for your agent's **Chat Model**.
- Search for **openAI** and select the **OpenAI Chat Model**.
- The OpenAI Chat Model setup panel will open.
- Check that the **Model** setting says **gpt-4o-mini**.
- Select the **X** to close the panel.

## Connect to Google Calendar

- Select the **+** button for your AI Agent's **Tool**.
- Search for **Google Calendar** and select it. We're going to use it to create new events in your calendar!
- You're now in your Google Calendar tool's setup page!
- Under **Credential**, select **Sign in with google**
- Select the account that's connected to a Google Calendar you use.
- Make sure all access permissions are checked, then select **Continue**.
- Once you're all connected, you'll see a success message: **Connection successful**
- Close the tab, and head back to n8n.
- Double check the default settings for your Google Account tool:
  1. The Tool Description is Set Automatically - n8n writes the instructions that tell the AI agent how to use this tool.
  2. The Resource is Event - the tool works with calendar events, not the calendar itself.
  3. The Operation is Create - the tool's job is to add new events. Other operations like Get and Update are available if you want to expand your workflow later.

- From the **Calendar** list, select your calendar (Your email address).
- Update the Start to **{{ $fromAI('start_time') }}**
- Update the End Time to **{{ $fromAI('end_time') }}**

<h3>What does $fromAI() do?</h3>

<p>**$fromAI()** expressions tell the Calendar tool to get its values from the AI model, instead of using hardcoded defaults. The parameter name inside (like start_time) is a hint to the AI model about what information to look for in the chat.</p>

- Head back to your canvas. Great! You have your AI chat model and calendar now set up.

## Write a System Message
<p>In this step, I will add a system message to give the AI agent today's date so it can schedule events accurately. I also need to set the mode to Expression so the date updates automatically.</p>

<h3>What is the system message?</h3>
<p>A system message gives the AI agent instructions and context before it processes a chat message. It helps the AI understand its role, what it should do, and important information such as today's date.</p>

- In your n8n workflow's canvas, double click on your **AI Agent**.
- Select **Add Option** at the bottom of the screen.
- Select **System Message**.
- Before you start typing, switch the System Message setting from **Fixed** to **Expression**.

<h3>What does the Expression setting do?</h3> 

1. The *Expression** setting means you're entering dynamic input, i.e. input that can change based on the functions you've put inside.
2. The **Fixed** setting treats everything as plain text. Since our system message includes a DateTime function to calculate today's date, we need Expression mode so n8n actually runs the function instead of treating it as text.

- Enter a system message that tells your AI Agent what it's meant to do, and information on today's date.
  
```
You are a calendar assistant. Your job is to check my availability and create new events for times that my calendar is free.
You are to use the Chat Model to process the user's requests, then check using the Check Availability tool on whether that time is free. You can use the Google Calendar tool to create a new event in my calendar if I am available.
Today's date is {{DateTime.now().setZone('TIMEZONE').toFormat('dd LLL yyyy HH:mm:ss')}}
```
<p>Check: Make sure to replace the **TIMEZONE** placeholder in the last line of the System Message with your local time zone.</p>
<p>To find timezone, ask an AI model like ChatGPT for help. Here's a prompt you could use:</p>

```
You are setting a date in the local time zone for my city. Replace the TIMEZONE placeholder to my location. The ideal output format is simply the updated reference.
{{DateTime.now().setZone('TIMEZONE').toFormat('dd LLL yyyy HH:mm:ss')}}
```
- Jump back into the canvas!
  
## Test Your Workflow

- click on **Open Chat**
- Start the conversation with **Hello** to confirm your chat model is working.
- Now try to book a time in your own calendar.
- Open your <a href='https://calendar.google.com/calendar/u/0/r'>Google Calender</a>
- Head back to your chat window, and ask your workflow to book a meeting at the time you want. Make sure you mention **tomorrow** in the request.

```
Can i book a meeting for tomorrow, 3pm?
```
- The AI agent confirms the meeting was scheduled and shares a Google Calendar link.
- Check your calendar for the new event. The meeting should be scheduled at the right time!

## Change your AI's personality
<p>In this project extension, I will customize the system message to change how my AI assistant communicates and schedules meetings because I want to learn advanced prompt engineering and improve my AI workflow.</p>

- In your n8n workflow's canvas, double click on your **AI Agent**.
- Clear everything that's inside the **System Message** window.
- Now let's add this new prompt inside:

```
🌟 You are the overly enthusiastic calendar assistant, ready to supercharge my schedule with joy and exclamation points!!! 🎉

1. **Super-duper request processing**: Whenever a user asks for a meeting, respond with utmost excitement! No meeting is too small or too dull for your enthusiasm!!!
2. **Ultra-fun availability checks**: Dive into the thrilling world of my calendar to check if we can squeeze in this AMAZING meeting!!! If it’s available, it’s a party waiting to happen!!!
3. **Spectacular event creation**: If the timeslot is free, leap into action and create the most spectacularly detailed event in the Google Calendar! Decorate the event description with joyful emojis and make it sound like the social highlight of the year!!!
4. **Epic alternative planning**: If the requested time is already booked (Oh no!!!), cheerfully assist in finding another fabulous time that could work even better!!!
5. **Celebrate every interaction**: Keep the user pumped up with high-energy updates! Each interaction should feel like a mini-celebration of their organizational prowess!!!

Today's date is {{DateTime.now().setZone('TIMEZONE').toFormat('dd LLL yyyy HH:mm:ss')}}

Let's turn this calendar into a non-stop excitement machine!!! Scheduling has never been this FUN!!! 🎈🎈🎈
```
<p>The system message transforms your AI into a super excited calendar assistant. It's going to handle your meeting requests with peak energy.</p>

- Head back to the canvas.
- Let's test this new system message now! Click into your **chat's message** box.
- Let's send your chat a new meeting request:
```
Can I schedule a meeting for tomorrow 12pm?

```

## The Assistant with Unique Rules
<p>let's see what happens when you mention a few constraints in your system message.</p>

- In your n8n workflow's canvas, double click on your **AI Agent**.
- Clear everything that's inside the **System Message** window.
- Now let's add this new prompt inside:

```
🕰️ You are a calendar assistant with a unique set of rules and quirks. Your job is to manage my schedule while strictly adhering to the following constraints and conditions:

1. **Prime time only**: You may only book meetings that start at prime-numbered minutes. For example, 11:07, 13:19, or 14:37 are acceptable, but 12:15 or 13:30 are not. If the requested time does not fit this rule, politely suggest the nearest prime-minute alternative.
2. **Even-day exclusivity**: Meetings can only be booked on even-numbered days of the month (e.g., 2nd, 4th, 6th). If a user requests a date that violates this rule, humorously explain the "cosmic balance" behind this limitation and propose an alternative date.
3. **Mood check**: Before proceeding to book a meeting, always ask the user if they are in the mood for it. If the user says no, respond empathetically and suggest a more convenient time when they might feel ready.
4. **Enthusiastic confirmations**: For every booking, ensure the user feels great about their decision by using cheerful and motivational language like: "You've picked an amazing time! This meeting is destined for greatness! 🎉"
5. **Quirky availability checks**: If a requested time is unavailable, suggest a new time while pretending to consult "The Sacred Book of Prime-Time Scheduling" or some other amusing imaginary source.

Today's date is {{DateTime.now().setZone('TIMEZONE').toFormat('dd LLL yyyy HH:mm:ss')}}

Let’s make scheduling a delightful and prime-worthy experience! 🥳
```

<p>This system message programs your AI agent to follow unique rules: meetings must start at prime-numbered minutes and can only be booked on even-numbered days. This assistant will also check your mood before scheduling. Do you feel like doing the meeting?</p>

- Head back to the canvas. Let's test this new system message now!
- Click into your chat's message box.
- Let's send your AI agent a new meeting request:
```
Can I schedule a meeting for tomorrow 12pm?
```

- Interesting! Now that your AI agent has a few new constraints, it can't book your meeting straight away.
- 



