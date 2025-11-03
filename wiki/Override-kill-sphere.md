In the stock game, a kill sphere is placed at an altitude at -250m, preventing you from diving anywhere under the surface of a gas giant, or any body.

This feature overrides it to a custom altitude, which allows for a lot more vertical space below the atmosphere/clouds/surface.

![Inside Jool](https://i.imgur.com/PL0iDOg.png)

For best effect you can combine this with Scatterer config points to make the atmosphere get progressively darker as the altitude gets lower or between cloud layers, especially since Scatterer atmos will look weird/broken when you go under them, so you can just make it fully dark then.

This feature is implemented in a simplistic manner and breaks and kills you if you try to go EVA below the stock kill sphere altitude, but that's good enough for gas giants.

## How to use

Add an overrideKillSphere node to your PQS_MANAGER OBJECT node with a custom altitude inside.

Example from my own Jool config:

`PQS_MANAGER
{
	OBJECT
	{
		body = Jool
		deactivateDistance = 175000
		overrideKillSphere
		{
			altitude = -75000
		}
	}
}`

