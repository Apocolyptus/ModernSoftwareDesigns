# Modern Software Designs
Week 3 - Topics and Resources


User Experience and Bad Prototyping on Modern Software Designs

When we think about user experience, we think about ways that we can improve it for the user. Often we will try to work in groups for the best concepts possible to create that experience that is flawless. It seems that a user experience is likened to an adventure. Sometimes it starts up just perfect, but the more you get into the adventure the worse off the experience becomes. This is because of the simple yet confusing link between user experiences: good and bad. What is a bad user experience? Seems like common sense when you think about it, but there are a multitude of ways that don't make sense. What about the ways that we can not improve it? What are ways that we can recognize bad user experiences?

I've found that it starts with the GUI. The graphical user interface is a necessary evil, but you need to also look at the UI, the other-other user interface: the database. Through the database, we can know exactly what the user will need. The best way to change someone's attitude about user interfaces is to show them users having problems with their product. Sometimes we create unnecessary solutions because of a failed communication with the end user. People value diversity and individuality but during a design process we assume that everyone is like us. 

To find the best user experience, you must actually turn off your computer, walk away from it and interact with real people. I know this tidbit of common sense is hard to comprehend, but the computer isn't the all fix in everything that we do. Writing and drawing the design out on paper will always be the way to design faster on the fly than any software ever. The best tools to have in a team are 1) pen and paper, 2) Balsamiq, Axure or InVision and 3) Prototyping in code. Remember, communication is the key to unlock the doors of the future. It is the most important part of any project. 

Virtual prototyping brings three key benefits: faster time-to-software-development, improved software quality, and lower development costs. To start code prototyping, I'll just create something in C#, as its a language I can write and somewhat understand what's going on. This user experience will be based on a pattern termed CommandPattern.


using System;
using System.Collections.Generic;
 
namespace CommandPattern
{
  public interface ICommand
  {
    void Execute();
  }
 
  public class Switch
  {
    private List _commands = new List();
    public void StoreAndExecute(ICommand command)
    {
      _commands.Add(command);
      command.Execute();
    }
  }
 
  public class Light
  {
    public void TurnOn()
    {
      Console.WriteLine("The light is on");
    }
 
    public void TurnOff()
    {
      Console.WriteLine("The light is off");
    }
  }
 
  public class FlipUpCommand : ICommand
  {
    private Light _light;
 
    public FlipUpCommand(Light light)
    {
      _light = light;
    }
 
    public void Execute()
    {
      _light.TurnOn();
    }
  }
 
  public class FlipDownCommand : ICommand
  {
    private Light _light;
 
    public FlipDownCommand(Light light)
    {
      _light = light;
    }
 
    public void Execute()
    {
      _light.TurnOff();
    }
  }
 
  internal class Program
  {
    public static void Main(string[] args)
    {
      Light lamp = new Light();
      ICommand switchUp = new FlipUpCommand(lamp);
      ICommand switchDown = new FlipDownCommand(lamp);
 
      Switch s = new Switch();
      string arg = args.Length &gt; 0 ? args[0].ToUpper() : null;
      if (arg == "ON")
      {
        s.StoreAndExecute(switchUp);
      }
      else if (arg == "OFF")
      {
        s.StoreAndExecute(switchDown);
      }
      else
      {
        Console.WriteLine("Argument \"ON\" or \"OFF\" is required.");
      }
    }
  }
}


This is a horrible user experience. Why? Everything is marked out as if a beginner went through and said "Okay if this is going to work, then this needs to happen... next.. If this is going to work..." and so on. The user experience here is a lot of lag and time for the object to interact with the end client. This is not good programming and somewhere along the line of communication there was a failure to either 1) understand or 2) understand. 

Why did I list understand twice? For the mere fact that understanding yet not understanding is not the same as understanding. You understand?

Going through the code, you can select the parts that you need and put them into one namespace. This fixes the bugs, errors, and lag. This now creates a better user experience. The top code has the motion that if you switch up the light is on, and if you switch down, the light is off. 


using System;
using System.Collections.Generic;
 
namespace CommandPattern
{
  public static class Program
  {
    private static readonly Dictionary Commands =
      new Dictionary
      {
        { "ON", () => Console.WriteLine("The light is on") },
        { "OFF", () => Console.WriteLine("The light is off") }
      };
 
    private static void Main(string[] args)
    {
      if (args.Length == 1 && Commands.ContainsKey(args[0]))
      {
        Commands[args[0]]();
      }
      else
      {
        Console.WriteLine("Argument \"ON\" or \"OFF\" is required.");
      }
    }
  }
}
