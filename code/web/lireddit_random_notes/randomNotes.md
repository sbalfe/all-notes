#### Watch mode

`tsc-watch` starts a TypeScript compiler with `--watch` parameter, with the ability to react to compilation status. `tsc-watch` was created to allow an easy dev process with TypeScript. Commonly used to restart a node server, similar to nodemon but for TypeScript.

https://stackoverflow.com/questions/59055799/tsc-not-creating-the-dist-folder/59056152

https://www.npmjs.com/package/tsc-watch

---

ctrl + . : add the import of red squiggle instantly

---

new file vscode > add name something/insidesomethingfolder

----

#### Persist and flush explanation , mikro orm entity manager.

https://mikro-orm.io/docs/entity-manager/#:~:text=persist(entity)%20is%20used%20to,be%20written%20to%20the%20database.&text=flush()%20will%20go%20through,and%20perform%20according%20database%20queries.

---

#### migrations

https://www.youtube.com/watch?v=JfIvPDPUFo4 - 

- migrations essentially just update our database automatically on changes defined in the entity.

---

#### https://typegraphql.com/docs/types-and-fields.html - type graphql explanation object type and fields.

---

https://typegraphql.com/docs/types-and-fields.html - types and fields of graphql typescript.

---

https://mikro-orm.io/docs/metadata-providers/ - read about meta data providers in mikrorm 

---

ORM creates a copy like react of the database, and only updates the table if some of schema has changed for example.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210731010819701.png)

- 

https://www.youtube.com/watch?v=bRAcWk9S-6g - decorators

https://stackoverflow.com/questions/40381401/when-to-use-saveuninitialized-and-resave-in-express-session - sessions variable explained.

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite - cookies same site shit

---

apollo studio on a different origin use postman to test for cookie presence as the session is seperate.

---

#### Sessions

```node
req.session.userId = user.id

{userId: 1} -> sends to redis server / mongo / postgres etc

sess: key -> {userId: 1}

express-session set a cookie on the browser key

when user makes request -> send key > server stored in the browser cookie data.

decrypt to key and then request to redis using the key to obtain that users data.

stores this data on req.session object

req.session: {
	userId: 1
}
```

----

#### next js

- the name of the file means something in next
- index react component refers to /
- any other name refers to /thatname in the origin instance.

https://stackoverflow.com/questions/59988667/typescript-react-fcprops-confusion - react and typescript functional components 

https://www.youtube.com/watch?v=3IdCQ7QAs38 - react props

---

https://chakra-ui.com/docs/features/style-props - style chakra ui elements, like react bootstrap.

----

#### Typescript interfaces vs types

https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces

---

### useField hook

**https://wanago.io/2020/03/30/forms-formik-react-hooks-api-typescript/**

https://stackoverflow.com/questions/52816623/graphql-post-request-in-axios - graphql axios

---

update urql cache when logging in again.

https://formidable.com/open-source/urql/docs/graphcache/

---

https://stackoverflow.com/questions/46677752/the-difference-between-requirex-and-import-x - import vs require.

---

### Pagination automatic

```tsx
import { withUrqlClient} from 'next-urql';
import { usePostsQuery } from "../generated/graphql";
import { createUrqlClient } from "../utils/createUrqlClient";
import React, { useEffect, useState } from "react";
import { Layout } from "../components/Layout";
import { Box, Flex, Heading, Link, Stack, Text } from "@chakra-ui/layout";
import NextLink from 'next/link'
import { Button } from "@chakra-ui/button";

const Index = () => {
    const [limValue, setLim] = useState(10);
    const [{data, fetching}] = usePostsQuery({
        variables :{
            limit: limValue
        },
    });

    /* load more when they reach end of screen  */
   useEffect(() => {
        const handleScroll = () => {
            console.log(window.innerHeight, window,pageYOffset, document.body.offsetHeight)
             if ((window.innerHeight + window.pageYOffset) >= document.body.offsetHeight) {
                setLim(prev => prev + 10);
            }
        }
        window.addEventListener("scroll", handleScroll, { passive: true });
        return () => window.removeEventListener("scroll", handleScroll);
    }, []);

    if (!fetching && !data){
        return <div> there is no data, posts are empty or query failed </div>
    }

    return(
        <Layout>
                <Flex align = "center">
                    <Heading> LiReddit</Heading>
                        <NextLink href = "/create-post">
                            <Link ml = "auto">create post</Link>
                        </NextLink>
                </Flex>
                {console.log("test")}    
                {/* display all posts if there are posts present and not fetching, */}
                <br /> 
                {fetching && !data ? (
                    <div>loading...</div>
                ): (
            
                <Stack spacing = {8}>
                    {data!.posts.map(p => (
                    
                    <Box key = {p.id} p={5} shadow="md" borderWidth="1px">
                    <Heading fontSize="xl">{p.title}</Heading>
                    <Text mt={4}>{p.textSnippet.slice(0, 50)}</Text>
                    </Box>
                ))}
                </Stack>
            )}
            {data ?(
                <Flex>
                    <Button my = {4}>Load More</Button>
                </Flex>
            ): null
            }
        </Layout>
    )
}


/*
    with  ssr:true > server side rendering enabled for this index page

    this means the request are made from the server before the page actually loads

    rather than just making requests from the browser itself.

    page time may take longer to load but when it does, no further request must be made.

    https://formidable.com/open-source/urql/docs/advanced/server-side-rendering/

    server side rendering enables google to pick up the results for SEO.

    only use for when you are performing some kind of query.
*/
export default withUrqlClient(createUrqlClient, {ssr: true})(Index);

```

