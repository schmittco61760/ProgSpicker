using System;
using System.Collections;

namespace schneider.loesung.exam.ws1516
{

    // Aufgabe 1

    class StackAndOrderdList<T> where T : class, IComparable
    {
        class Element
        {
            public T Data;
            public Element Next;

            public Element(T Data, Element Next = null)
            {
                this.Data = Data;
                this.Next = Next;
            }

        }
        private Element stackStart = null;
        private Element sortedStart = null;

        public void Add(T Value)
        {

            this.stackStart = new Element(Value, this.stackStart);

            Element preRunner = null;
            Element runner = this.sortedStart;

            while (runner != null && (runner.Data.CompareTo(Value) < 0))
            {
                preRunner = runner; runner = runner.Next;
            }


            if (preRunner == null)
                this.sortedStart = new Element(Value, this.sortedStart);
            else
                preRunner.Next = new Element(Value, runner);
        }

        public T this[int index]
        {
            get
            {
                int count = 0;
                Element temp = this.stackStart; 
                while (temp != null) 
                {
                    if (count == index)
                        return temp.Data;
                    count++; 
                    temp = temp.Next;
                }
                throw new IndexOutOfRangeException();
            }
        }

        private IEnumerable getEnumerator(Element current)
        {
            while (current != null)
            {
                yield return current .Data;
                current = current.Next;
            }
        }


        public IEnumerable GetStackEnumerator()
        {
       
            return this.getEnumerator(this.stackStart);
        }
    
        public IEnumerable GetSortedEnumerator()
        {

            return this.getEnumerator(this.sortedStart);
        }


    }



    class MainClass
    {
        public static void Main(string[] args)
        {
            StackAndOrderdList<String> list = new StackAndOrderdList<string>();
            list.Add("b");
            list.Add("c");
            list.Add("a");

            foreach (String current in list.GetStackEnumerator())
            {
                Console.WriteLine(current);
            }

            foreach (String current in list.GetSortedEnumerator())
            {
                Console.WriteLine(current);
            }
        }
    }
}
