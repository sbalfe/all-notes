# Tech Stack for shrillecho-app

- Nginx **[docker container A]**
  - reverse proxy requests to Next.js server **container B** **[shrillecho.app]**
  - reverse proxy requests to gunicorn flask HTTP server **container C** **[api.shrillecho.app]**

### Frontend (Next.js)

- Next.js (React framework that requests nginx **container A** for **api.shrillecho.app** ) + Typescript **[docker container B]**
  - Has a node.js server for server side rendering of page content / assets to supply to client.
- GraphQL client (apollo)

### Backend (Flask)

- Flask (gunicorn wsgi HTTP server serving flask requests **container D** for its data store and **container E** for caching/sessions)  **[docker container C]**
- MongoDB (noSQL document store ideal for unstructured JSON ) **[docker container D]**
  - Maybe mongoengine ORM
  - Likely integrated for now into shrillecho-lib
- Redis **[docker container E]**
  - caching 
    - mongoDB responses
    - 3rd party api responses (Spotify etc.) - Integrated into shrillecho-lib likely
  - Sessions
- GraphQL server (graphene)

> Other considerations:
>
> - Cloud - scalability and ease of hosting
>   - Firebase (document store database and various other functions)
>   - GCP (cloud functions)
>   - AWS (dynamo DB and Elastic compute)
>   - Azure (Azure VMs)

##### Dependencies

- shrillecho library