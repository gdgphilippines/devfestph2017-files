# 5. Best Practices

### Action naming and policies

*   There are some policies about what a Conversation Action can be named and support.
*   Anything targeting children under 13 is not allowed. Also, there are a variety of categories which are prohibited: Health, gambling, harassment, violence, hate speech, etc.
*   Need to respect intellectual property concerns.
*   Only Conversation Actions, with a well defined invocation, are supported. The guidelines explain all the rules around them.

### Conversation Design

Please remember that in order to make a really good Action, you need to think carefully about the design.

Designing a spoken dialog between a human and a computer - in advance - accounting for all the possibilities, both in function and user behavior, and still have it feel natural - is harder than it looks.

The **key to building a good voice interface** is to not fall into the trap of simply converting a graphical user interface into a Voice User Interface. This defeats the purpose of using a conversation. People are not going to change how they talk anytime soon, so take what we know about human-to-human conversation to **teach our computers to talk to humans. Not the other way around.**

#### Top 3 Design Tips

#### 1.  Create a persona. 
  *  It's the consistent character, captured by the voice and interactive experience. It's the face of the experience for the user, who they will be talking to. Persona It is conveyed through:

  1.  Tone
  2.  Word and phrase choices
  3.  Style
  4.  Technique
  5.  Voice

  The persona should be based on: Your user population and their needs and the imagery & qualities associated with your agent's brand.

#### 2.  Think outside the box, literally. 

  You should write your core experiences like you would a screenplay. This can be as scrappy as acting it out with a colleague and documenting it on paper, or creating an interactive wizard-of-oz prototype you tweak and play with until you're ready to start coding.

  ![Prototype Image](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/2c1ff83450f8cb0a.png)

  And then, when you draw out your initial vision, keep it at a high level, where the boxes represent entire dialogs or user intents, but leave out the individual wording you'll use in the interaction.

  ![Flowchart Image](https://codelabs.developers.google.com/codelabs/your-first-kids-action-on-google/img/e6de757654276287.png)

#### 3. In conversations, there are no "errors"

  When a problem happens, imagine what the user hears when your action says "I don't understand YOU".

  This is one of the greatest cause of user frustration and aversions to voice interfaces. People get angry and raise their voice and repeat the same answer again!

  We need to give people credit for knowing how to speak. Just because they don't understand a prompt or choices, doesn't mean they don't speak the language. So help them be successful.

  And remember what happens along the way, maintain context.

  The best way to course-correct in advance while still maintaining a natural, comfortable conversation, is to plan for these occurrences as if they were any other turn in a conversation - i.e. to treat them as input that didn't cause the "error" in and of themselves.

  We have [a great set of design materials](http://g.co/dev/ActionsDesign?kids-codelab=1) on our developer site to help you think about how to design better conversations.