## Synopsis

the purpose of this MSPL script is to let Lync handle incoming phone calls and forward them to the chosen SIP contact instead of the original person being called.

## Code Example

IT's an MSPL script, check out this blog post I put up with useful MSFT links if you need to get familiar with the moving parts: https://gregbesso.wordpress.com/lync-skype-for-business/call-forwarding-with-mspl-scripts/

## Motivation

Needed to respond to a request to forward certain incoming calls, so did some research and found this way to get it done.

## Installation

Create a new network shared folder, where you will store your MSPL script files. Somewhere that the Lync servers will be able to access.

Create a new empty text file in that share, and rename it to be something like CallForwarding.am.

Fill this in with your script contents, which you can use the example from Microsoft to get started (https://msdn.microsoft.com/en-us/library/office/dn439073.aspx)
In the Microsoft example, look for the toUri = … line and the IF loop below it. Replace that section with something like…
toUri = GetUri(sipRequest.To);
toUserAtHost = Concatenate(GetUserName(toUri), “@”, GetHostName(toUri));
telNum = GetUserName(toUri);
if ((ContainsString(toUserAtHost, “+12125551212@YourDomain.com”, true)) ){   Respond(“302?,”Moved Temporarily”,”Contact=<sip:ForwardCallsToThisPerson@YourDomain.com>”);    return;}
*That last line, the If statement one, you can copy/paste that for as many different # forward scenarios you want to handle. Each phone # you forward gets its own If statement line.

Create a new server application on your Lync pool, by running something like the following from the Lync Management Shell…
New-CsServerApplication –Identity “registrar:YourPool.YourDomain.com/CallForwarding” –Uri http://YourDomain.com/CallForwarding -ScriptName “\\Server\Share\LyncFiles\CallForwarding\CallForwarding.am” –Critical $False –Enabled $False

Enable this new server application from the Lync admin console in the browser. Click Topology > Server Application, then highlight this new item you created and click Actions > Enable.

## API Reference

No API here. <sounds of crickets>

## Tests

No testing info here. <sounds of crickets>

## Contributors

Just a solo script project by moi, Greg Besso. Hi there :-)

## License

Copyright (c) 2015 Greg Besso

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.