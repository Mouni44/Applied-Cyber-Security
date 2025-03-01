# WEEK 2 Assignment

## Research Topics :

### HTTP Methods:
  The set of common methods for HTTP/1.1 is defined below ad this set can be expanded based on requirements. These method names are case sensitive and they must be used in uppercase.

  Methods and Description 
    GET method : The GET method is used to retive information from the given server using a given URi. request using GET should only retrive data ad should have no other effect on the data.

    POST method : Submits data to processed to a specified resources. It's often used to create new entities or send data to a server to update a resource's state.

    PUT method : Updates a resource at a specified URI. It replaces the curret representation of the target resources with the request payload.

    DELETE method : Removes a specified resource.

    PATCH method : Applies partial modifications to a resource. It's used to apply minor updates to a resource.

    HEAD method : Similar to GET but retrives only the headers and not the body of the response. It's often used to check for the exstence or metadata of a resource.

    OPTIONS methos : Requests information about the communication options available for the target resource. It's used to describe the communication options for the target resource.

    TRACE method : Echoes back the received request to the client, which is useful for debugging or diagnostics.

    CONNECT method : Establishes a tunnel to the server identified by a given URI for the purpose of communication over a secure connection.

  These methods defined the action to be performed on a resource when a client communicates with a server via HTTP, allowing for various operations like retrieval, creation, modification, and deletion of resources.

### Hashing Mechanisms
   Hashing Mechanisms are cryptographic processes that convert input data (of any size) into a fixed-size string of characters, typically a hash value or digest. These mechanisms are widely used in various field for security, data integrity verification, and indexing. Here are some common hashing mechanisms.

   1. MD5 (message Digest Algorithm 5) : It produces a 128-bit hash value and was widely used previously. However, due to vulnerabilities and collision attacks discovered over time, it's now considered weak and not recommended for security purposes.

   2. SHA-1 (Secure Hash Algorithm 1) : initially designed to produce a 160-bit hassh value, SHA-1 has also been found to have vulnerabilities and is no longer considered secure for critical applications.

   3. SHA-2 (Secure Hash Algoritm 2) : This family includes SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, and SHA-512/256. SHA-256 and SHA-512 are widely used and are considered secure for various cryptographic applications.

   4. SHA-3 (Secure Hash Algorithm) : It's the latest member of the SHA family standardize by NIST. SHA-3 offers different internal workings compared to SHA-2 and is designed to provide a high levle of security.

   5. Bcrypt : it's password hashing function based on Blowfish cipher and is designed to slow down brute-force attacks by requiring more computational power and time to hash passwords. It's commonly used for securely storing passwords.

   6. Argon2 : Winner of the Password Hashing Competition, Argon2 is a memory-hard function that's resistant to both GPU and ASCI attackes. It's designed to protect against password cracking and hash collision attacks.

   7. CRC (Cyclic Redundancy Chech) : While not a cryptographic hash function, CRC is a widely used method for error-checking. It generates a fixed-size checksum based on input data and is often used in data transmission to detect errors.

   Hashing Mechanisms play a vital role in ensuring data integrity, securely storing passwords, digital signatures, and various security-related applications. The choice of hash function depends on specific security requirements and the level of vulnerability to attacks prevalent at a given time.

###  Rubber duck debugging
   Rubber duck debugging is an informal technique used by programmers to solve problems. the idea is simple : explain the code or the issue step-by-step to an inanimate object, lika a rubber duck. By articulating the problem aloud, the process of verbalizing often helps the programmer to understand the issue better and find a solution.
   Here's how it typically works-- 

   1. Grab a Rubber Duck (or any inanimate object) : The programmer explains the code or the problem they're facing to the rubber duck as if the duck is unfamiliar with the code or the issue.

   2. Describe Step-by-Step : The programmer goes through the code or the problem, line by line or step by step, explaining what each part does what they expect it to do.

   3. Articulate the issue : While explaining, often the person might realize where the problem lies or what's not working as expected. the act of articulating the problem can trigger insights or revelations about potential mistakes or oversights.

   4. Find Solutions : By talking through the problem, the programmer might identify the issue and come up with potential solutions Sometimes, just the act of talking it out is enough to see the problem and fix it.

   It might sound silly, but the process of explaining the issue to an external entity (even an inanimate object) can help in arganizing patterns, and discovering solutions that might not have been apparent before. the goal is to force oneself to think more thoroughly about the problem, often to solving it independently.

### Robots.txt
   The 'robots.txt' file is a text file webmasters create to instruct web robots (usually search engine crawlers) how to crawl and index pages on their website. It's standard used by websites to communicate with web crawlers and specify which parts of the site should be crawled or not.
   Key components of the 'robots.txt' file :

   1. User-agent : This indicates the specific web crawler to which the rules apply. For example, 'user-agent: Googlebot' refers to google's crawler, while 'user-agent: *' applies to all crawlers.

   2. Disallow : this specifies the directories or files that should not be crawled. For instance, 'Disallow: /privete/' would prevent crawlers from accessing the '/private/' directory.

   3. Aloow : This directory can be used to averride specific 'Disallow' directives. For instance, 'Allow: /public/' could permits access to the '/public/' directory even if there's a broader 'Disallow' rule.

   4. Sitemap : This line specifies the location of the XML sitemaps file for the website. For example, 'Sitemap" https//www.example.com/sitemap.xml' .

    Here's an example of a simple 'robots.txt' file : 

            User-agent: *
            Disallow: /private/
            Disallow: /confidential/
            Allow: /public/
            Sitemap: https://www.example.com/sitemap.xml

    It's important to note that while most reputable search engine crawlers honor the directives in 'robots.txt' , it's not a way to enforce security or prevent access to sensitive data. it's more of a guideline for well-behaved web crawlers.

However, if there's sensitive information or content that you absolutely don't want to be accessed publicly, relying solely on 'robots.txt' might not be sufficient. Using other access control methods and security measures is crucial to protect such content.

### Sitemap.xml
   A 'sitemap.xml' file is a structured XML file that lists the URLs of a website's pages. It's used by search engines to crawl and index the site more efficiently. The primary purpose of a sitemap is to provide search engines with information about the site's organization, the most important pages, and how often they are updated.
   Key poins about 'sitemap.xml' :

   1. URLs Listing : The file contains a list of URLs for a website's pages. These URLs may include information about their priority, last modification date, change frequency, and relationship to other URLs.

   2. Helps Search Engine Crawlers : Search engine crawlers (like Googlebot) use sitemaps to discover and index pages more efficiently. While it doesn't gurantee inclusion or ranking, it provides valuable information for search engines to understand the wedsite's structure.

   3. Improves Indexing : Particularly helpful for large websites, new websites with few external skills, or sites with dynamically generated content where some pages might not be easily discovered by search engines through normal crawling processes.

   Here's a simple example of a 'sitemap.xml' file structure :

    ```htm
     <?xml version="1.0" encoding="UTF-8"?>
     <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
       <url>
         <loc>https://www.example.com/page1</loc>
         <lastmod>2023-12-20</lastmod>
         <changefreq>weekly</changefreq>
         <priority>0.8</priority>
       </url>
       <url>
         <loc>https://www.example.com/page2</loc>
         <lastmod>2023-12-18</lastmod>
         <changefreq>monthly</changefreq>
         <priority>0.6</priority>
       </url>
       <!-- More URLs -->
     </urlset>
     ```

   Explanation :
   - '< loc >' : Represents the URL of a specific page.
   - '< lastmod >' : Indicates the last modification date of the page
   - '< changefreq >' : Specifies how frequently the page changes (e.g., 'always', 'hourly', 'daily', 'weekly', 'monthly', 'yearly', 'never').
   - '< priority >' : Provides a hint to search engines about the priority of a URL relative to other URLs on the site (typically a value between 0.0 and 1.0).

   Having a well structured 'sitemap.xml' can aid search engine crawlers in efficiently discovering and indexing content, potentially improving a website's visibility in search engine results.

   ### SEO (Search Engine Optimization) : 
   SEO, or Search Engine Optimization, refers to the practices and techniques used to improve a website's visibility and ranking in search engine results pages (SERPs). The goal of SEO is to increase organic (non-paid) traffic to a website by making it more visible to people searching for relevant information, products, or services.

   Key elements and strategies involved in SEO:

   1. Keyword Research: Identifying and targeting the right keywords that users are likely to use in search queries. This involves understanding search intent and choosing keywords relevant to your content.

   2. On-Page Optimization: Optimizing individual pages on a website for specific keywords. This includes optimizing meta tags (title, description), using relevant headers, incorporating keywords in content, improving URL structure, and optimizing images.

   3. Quality Content: Creating high-quality, informative, and engaging content that provides value to users. Content should be well-written, relevant, and address the needs and queries of the target audience.

   4. Technical SEO: Ensuring the website is technically sound and easily accessible to search engine crawlers. This involves optimizing site speed, mobile-friendliness, fixing broken links, improving site architecture, using structured data, and optimizing robots.txt and sitemap.xml files.

   5. Link Building: Acquiring high-quality backlinks from reputable and relevant websites. Backlinks are an important ranking factor for search engines and can significantly impact a site's authority and visibility.

   6. User Experience (UX): Providing a positive user experience by ensuring easy navigation, fast-loading pages, mobile responsiveness, clear call-to-actions, and a user-friendly interface.

   7. Local SEO: For businesses targeting a local audience, optimizing for local search is crucial. This involves creating and optimizing Google My Business listings, obtaining local citations, and garnering positive reviews.

   8. Analytics and Monitoring: Using tools like Google Analytics and Google Search Console to track website performance, monitor traffic, analyze user behavior, and make data-driven decisions for further optimization.

SEO is an ongoing process that requires continuous monitoring, adaptation to algorithm changes, and refinement of strategies to stay competitive in search engine rankings. While it's complex and multifaceted, effective SEO practices can significantly enhance a website's visibility and bring in valuable organic traffic.





# Assignment 

Explain the given topics

- IP Addresses :
  IP (Internet Protocol) addresses are numerical lables assigned to devices connected to a computer network. They serve two primary purposes : Identifying the host or network interface and providing the location of the device in a network.

       Examples :

        IPv4 Address : 198.168.1.10
        IPv6 Address : 2001:0db8:85a3:0000:0000:8a2e:0370:7334

- Types of IP Addresses (IPv4 and IPv6) :
  Ipv4 (Internet Protocol version 4 ) :
    - Uses a 32-bit address format.
    - Written in the form of four decimal numbers aeparated by dots.
    - Addresses are running out due to limited number of unique addresses available.
      
      Example :

       IPv4 Address : 172.16.254.1

  IPv6 (Internet Protocol version 6) :
    - Uses a 128-bit address format.
    -Written in HexaDecimal notation with 8 groups of four HexaDecimal digits separated by colons.
    - Provides a vastly larger poor of addresses compared to IPv4.

      Example : 

       IPv6 Address : 2001:0db8:85a3:0000:0000:8a2e:0370:7334

- IP Routing (Classful and CIDR):
  1. Classful Routing:
      - Classful addressing divides IP addresses into classes (Class A, B, C, D, and E).
      
      - Based on the first few bits of the address, it determines the network and host portions.
      
      - Became inefficient due to the waste of IP addresses.

  2. CIDR (Classless Inter-Domain Routing):
      - Introduced to address the inefficiencies of classful routing.
      
      - Allows for more flexible allocation of IP addresses by using variable-length subnet masking (VLSM).
      
      - Employs a prefix notation that includes both the network address and the number of significant bits in the subnet mask (e.g., 192.168.1.0/24).

        Examples : 

         Classful Routing Example:
           Class A: IP addresses from 0.0.0.0 to 127.255.255.255
           Class B: IP addresses from 128.0.0.0 to 191.255.255.255
           Class C: IP addresses from 192.0.0.0 to 223.255.255.255
         CIDR Example:
           CIDR Notation: 192.168.1.0/24 (This represents a subnet with a 24-bit network part)

- CIDR - Subnets, Subnet masks, Gateways, Broadcast Addresses:
    1. Subnets:
     - Subnets are logical divisions of an IP network. 
     - They allow for segmentation, enabling better network management and security.
     - They help reduce network traffic by breaking a large network into smaller segments.

             Example :
               Original Network: 192.168.0.0/24
               Subnet 1: 192.168.1.0/25
               Subnet 2: 192.168.1.128/25
    
    2. Subnet Masks:
     - Subnet masks determine the network and host portions of an IP address.
     - Expressed in binary as a series of ones (representing the network) followed by zeros (representing the host).

             Example :
               Subnet Mask for /24: 255.255.255.0 (24 bits for network, 8 bits for hosts)
               Subnet Mask for /25: 255.255.255.128 (25 bits for network, 7 bits for hosts)
    
    3. Gateways:
     - Gateways (routers) are devices that connect different networks together.
     - They facilitate the routing of data between different networks.

             Example :
               Gateway IP: 192.168.1.1 (This is the gateway/router for the subnet 192.168.1.0/24)
    
    4. Broadcast Addresses:
     - Broadcast addresses are used to send data to all devices on a specific network.
     - In IPv4, the broadcast address is the highest address in the subnet. 
     - For example, in the subnet 192.168.1.0/24, the broadcast address is 192.168.1.255.

             Example :
               Broadcast Address for /24: 192.168.1.255
               Broadcast Address for /25: 192.168.1.127 (for subnet 1), 192.168.1.255 (for subnet 2)









