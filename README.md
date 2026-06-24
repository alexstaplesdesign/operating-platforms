# CS 230 - Operating Platforms
## Portfolio Artifact: Software Design Document — The Gaming Room

### About the Client and the Project

The Gaming Room is a client that contracted Creative Technology Solutions to expand their existing Android game, Draw It or Lose It, into a browser-based web application accessible across multiple operating systems. The game is a drawing and guessing competition where teams play through four-round sessions with timed windows for each guess. The client's core requirements were straightforward: support multiple teams per game, multiple players per team, enforce unique game and team names, and ensure only one game instance could exist in memory at any given time.

### What I Did Well

The platform evaluation and recommendations sections are the strongest parts of this document. The evaluation table breaks down server-side hosting, client-side support, and development tools across Linux, Mac, Windows, and mobile in a way that actually connects each platform's characteristics to what the client needed, not just a generic feature list. The recommendations section goes deeper than a surface-level OS comparison. It works through operating system architecture, storage management, memory management, distributed systems, and security as separate but connected concerns, all tied back to the specific constraints of the game. Flagging that macOS Server was discontinued in 2022 and documenting that with a citation rather than just saying "Mac doesn't work" is the kind of detail that makes a design document credible.

### How the Design Document Helped During Development

Working through the domain model before writing code made the application structure a lot clearer. Having the inheritance chain from Entity down to Game, Team, and Player laid out in the UML diagram meant the class hierarchy was not something to figure out mid-implementation. The singleton and iterator patterns were identified early as the right solutions for two specific requirements: keeping a single GameService instance in memory and preventing duplicate names. Writing those decisions into the design document first meant the code had a clear target instead of being reasoned out on the fly.

### What I Would Revise

The section I'd go back and strengthen is the recommendations. The design constraints section raises WebSockets as worth considering for the drawing and guessing portions of the game, since those are time-sensitive and REST adds overhead that could matter in a one-minute round. But the recommendations never circle back to it and stay focused on REST without explaining why WebSockets were ruled out or deprioritized. That loose end stands out. On a smaller scale, the iterator pattern gets less attention than the singleton throughout the document. The singleton gets a full explanation of how it works and why it satisfies the client requirement. The iterator is accurate but brief. Bringing those two up to the same level of detail would make the document feel more balanced.

### Interpreting User Needs

The client's stated requirements were concise, but the design work required reading into what they actually meant in practice. "Only one game instance in memory at a time" is easy to satisfy in a single-user context, but it is a real concurrency problem in a web environment where hundreds of requests might arrive simultaneously. Recognizing that distinction was what led to documenting the singleton pattern as a design constraint rather than just an implementation detail. The same logic applied to unique naming. In a web environment, two users could try to claim the same team name at the same moment, so validating against a single authoritative list is not optional. User needs that look simple on paper often carry technical complexity once you think through the environment the software will actually run in.

### Design Approach and Future Strategies

The approach here was to start with the object-oriented principles that structure the application, document the design patterns that address specific constraints, and then evaluate the deployment environment against what the application actually needs. Inheritance, encapsulation, composition, and the singleton and iterator patterns were identified first because the application's requirements pointed directly to them. Platform evaluation came after, because you need to understand what you are building before you can decide where it should run. Going forward, I would keep that sequence and add a formal architecture diagram earlier in the process. The written recommendations in this document are strong, but a topology diagram done in parallel would have made the tradeoffs between CDN delivery, server load balancing, and database design easier to communicate to both the client and a development team.
