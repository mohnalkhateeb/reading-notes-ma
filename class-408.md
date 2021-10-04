# OO Design
* ## Don't Repeat Yourself(DRY)
    is a principle of software development aimed at reducing repetition of software patterns,replacing it with abstractions or using data normalization to avoid redundancy.

    - When the DRY principle is applied successfully, a modification of any single element of a system does not require a change in other logically unrelated elements. 
    -  elements that are logically related all change predictably and uniformly, and are thus kept in sync.

* #### Alternatives of DRY
    - ###### WET (write everything twice)
        WET solutions are common in multi-tiered architectures where a developer may be tasked with, for example, adding a comment field on a form in a web application. The text string "comment" might be repeated in the label, the HTML tag, in a read function name, a private variable, database DDL, queries, and so on. A DRY approach eliminates that redundancy by using frameworks that reduce or eliminate all those editing tasks except the most important ones, leaving the extensibility of adding new knowledge variables in one place.
    - ###### AHA (Avoid Hasty Abstractions)
        AHA is rooted in the understanding that the deeper the investment we've made into abstracting a piece of software, the more we perceive that the cost of that investment can never be recovered (Sunk cost fallacy). Thus, engineers tend to continue to iterate on the same abstraction each time the requirement changes. AHA programming assumes that both WET and DRY solutions inevitably create software that is rigid and difficult to maintain. Instead of starting with an abstraction, or abstracting at a specific number of duplications, software can be more flexible and robust if abstraction is done when it is needed, or, when the duplication itself has become the barrier and it is known how the abstraction needs to function.

* ## Rule of three (computer programming)
    - **Rule of three** ("Three strikes and you refactor") is a code refactoring rule of thumb to decide when similar pieces of code should be refactored to avoid duplication. It states that two instances of similar code do not require refactoring, but when similar code is used three times, it should be extracted into a new procedure. The rule was popularised by Martin Fowler in Refactoring and attributed to Don Roberts.

    - **Duplication** is considered a bad practice in programming because it makes the code harder to maintain. When the rule encoded in a replicated piece of code changes, whoever maintains the code will have to change it in all places correctly.

* ## You aren't gonna need it(YAGNI)
    - **YAGNI** is a principle of extreme programming (XP) that states a programmer should not add functionality until deemed necessary.
    - **YAGNI** is a principle behind the XP practice of "do the simplest thing that could possibly work" (DTSTTCPW).
* ## Minimum Viable Product (MVP) 
    is a version of a product with just enough features to be usable by early customers who can then provide feedback for future product development.
    A focus on releasing an MVP means that developers potentially avoid lengthy and (ultimately) unnecessary work. Instead, they iterate on working versions and respond to feedback, challenging and validating assumptions about a product's requirements.
     - ###### Purposes
        - Be able to test a product hypothesis with minimal resources
        - Accelerate learning
        - Reduce wasted engineering hours
        - Get the product to early customers as soon as possible
        - Base for other products
        - To establish a builder's abilities in crafting the product required
        - Brand building very quickly

    -  ###### Business Model Canvas
        The Business Model Canvas is used to map in the major components and activities for a company starting out.
        ![BMC](https://businessmodelanalyst.com/wp-content/uploads/2021/05/Canva-Business-Model-Canvas-1024x576.jpeg)




