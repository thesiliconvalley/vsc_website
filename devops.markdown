---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
title: "VALLEY STARTUP CONSULTANT"
description: "VALLEY STARTUP CONSULTANT"
---
{% include sections/hero-gradient.html 
  h1="DevOps"
  h2="Streamlining Your Cloud <br><strong>Infrastructure</strong>"
  h2-class="two-line"
  span=""
  cta=true
  imgkit-url="/assets/img/devops-page.svg"
  alt-text=""
  left-col-width="col-md-5"
  img-col-class="col-md-7 col-offset-1" 
  hero-class="gradient-hero-with-logos legal-services-hero"
  img-width=590
  show-background-triangle=true
  show-background-ovel=true  
  container-class="hero-container"  
  show-background-triangle=true
  left-col-class="devops"
  class="img-fluid"
%}
<section class="section justify-content-end justify-content-lg-center blue-gradient heading">
  <div class="container">
    {% include sections/section-heading.html
      clients=false
      stats= false
      heading-top=true
      heading="Platform Engineering Services"
      description="In today's global arena, secure & scalable platforms are mission-critical. Platform engineers design, build, and manage resilient infrastructure & tools for your software applications. We deliver enhanced security, fault tolerance, and elastic scalability, perfectly aligned with your business objectives."
      heading-cta=flase
      headingCTA-type = ""
      cta-url="linkdddd"   
      class="blue-gradient heading stats-section "      
    %}   
     {% include sections/engineering-services.html
            class="engineering-services"
            %} 

</div>
  </section>
{% include sections/sections.html
      clients=false
      heading-top=true
      heading="More than 7,00 Clients <br> trust VSC in the world"
      description=""
      heading-cta=flase
      headingCTA-type = ""
      cta-url=""
      alt-text="testing"
      img-url="map.svg"
      backup-img=""
      class="testimonials"
      section-id ="testimonials"
      Type="testimonials"
    %}   
{% include sections/sections.html
      clients=false
      heading-top=true
      heading="Blog"
      description="Browse articles"
      heading-cta=true
      headingCTA-type = "button"
      cta-url=""
      alt-text=""
      img-url=""
      backup-img=""
      class="red-blue-gradient heading bg-left-blue blog"
      section-id ="blog"
      Type="blog"
    %}

    
{% include sections/sections.html
      clients=false
      heading-top=true
      heading="Contact Us"
      description="Have an inquiry? We’ll be happy to assist you"
      heading-cta=true
      headingCTA-type = "button"
      cta-url=""
      alt-text=""
      img-url=""
      backup-img=""
      class=" contact"
      section-id ="contact"
      Type="contact"
    %}
    


