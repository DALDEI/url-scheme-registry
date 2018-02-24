Forked from valdar/url-scheme-registry forked from brianm/url-scheme-registry
Converted to Gradle build

Library to make it easy to register new URL schemes for java.net.URL.
Consider:

    @Test
    public void testRegisterHandler() throws Exception
    {
        UrlSchemeRegistry.register("dinner", DinnerHandler.class);

        assertThat(read(new URL("dinner://steak"))).isEqualTo("steak");
    }

which uses the URL handler:

    public class DinnerHandler extends URLStreamHandler
    {
      @Override
      protected URLConnection openConnection(URL u) throws IOException
      {
        final String breakfast = u.getHost();
        return new URLConnection(u)
        {
          @Override
          public void connect() throws IOException { }

          @Override
          public InputStream getInputStream() throws IOException
          {
              return new ByteArrayInputStream(breakfast.getBytes());
          }
        };
      }
    }

The library uses the <code>java.protocol.handler.pkgs</code> system
property and runtime generated classes following the correct package
and class name conventions to accomplish this. It does *not* use
<code>URL.setURLStreamHandlerFactory(...);</code> for this (so code
using this library should run fine inside Tomcat which does use that
method).

Releases are distributed via jcenter https://bintray.com/daldei/maven/url-scheme-registry

Gradle:

    repositories {
       jcenter()
    }
    dependencies {
      compile 'com.calldei:url-scheme-registry:1.0'
    }

Good luck!
