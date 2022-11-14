<template>
    <Page>
        <ActionBar>
            <Label text="Home"/>
        </ActionBar>
        <StackLayout>
            <StackLayout>
                <Button text = "->" @tap = "Next()" />
                <Button text = "<-" @tap = "Back()" />
            </StackLayout>
                <StackLayout class="stack" v-for="Name,index in monthsNames" :key="index" v-if="Name === Months.Name">
                    <label>{{Name}}</label>
                </StackLayout>
                <StackLayout v-for="d in 7" :key = "d">
                    <StackLayout>
                        <label v-for="qwe in DayInWeek.length" v-if="WeekDays(d)">{{DayInWeek[qwe]}}</label> 
                    </StackLayout>
                </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>
  export default {
    data() {
        return {
            monthsNames: ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],
            Days: [1, 2, 3, 4, 5, 6, 0],
            Data: new Date(),
            Months:               {   
                    Name: '', //название месяца
                    Number: '', //номер месяца начиная с 0
                    NOD: '', //кол-во дней в месяце
                    Year: '' //год 
                } ,
            Count: 0,
            monthdays:[],
            DayInWeek:''
        };
    },
    
    computed:{
    },
    methods:{
        calculatemd()
        {
            this.monthdays = []
            for (let i = 1; i < this.Months.NOD + 1; ++i)
            {
                this.monthdays.push({day:i,
                DayOfWeek: new Date(this.Months.Year,this.Months.Number,i).getDay(),
                DayInWeek: this.DayInWeek
                })
            }
            console.log(this.monthdays)
        },
        WeekDays(j)
        {
            for (let i = 1; i < this.Months.NOD + 1; ++i)
            {
                if(this.monthdays.DayOfWeek == j)
                {
                    this.DayInWeek.push(this.monthdays.DayOfWeek) 
                }
            }
            console.log(this.DayInWeek)
        },
        fnday(day)
        {
            let num = new Date(this.Months.Year,this.Months.Number,day).getDay()
            return num 
        },
        fnfirstweek(day) //функция должна помочь вывести первую неделю в месяце, а затем в зависимости от Count выведутся последующие днgи
        {
            let status = null
            for(let i = 1; i < day+1;++i)
            {
               if(this.Days[day - 1] === this.fnday(i))
               {
                status = true
                this.Count++
                break
               }
               else
               {
                status = false
               }
            }

            return status
        },
        fnmonthday(y,m)
        {
            let name = ((new Date(y,m).toLocaleDateString('default', {month:'short'})).trim().split(" "))[1]
            return name
        },
        fnNOD(y,m)
        {
            let nod = new Date(y,m + 1,0).getDate()
            return nod
        },
        Next()
        {
            this.Count = 0
            if (this.Months.Number == 11)
            {
                this.Months.Number = 0
                this.Months.Year++
                this.Months.Name = this.fnmonthday(this.Months.Year,this.Months.Number)
                this.Months.NOD = this.fnNOD(this.Months.Year,this.Months.Number)
            }
            else
            {
                this.Months.Number++
                this.Months.Name = this.fnmonthday(this.Months.Year,this.Months.Number)
                this.Months.NOD = this.fnNOD(this.Months.Year,this.Months.Number)
            }
            this.calculatemd()
            console.log(this.Months) //можно увидеть что данные в объекте меняются
        },
        Back()
        {
            this.Count = 0
            if (this.Months.Number == 0)
            {
                this.Months.Number = 11
                this.Months.Year--
                this.Months.Name = this.fnmonthday(this.Months.Year,this.Months.Number)
                this.Months.NOD = this.fnNOD(this.Months.Year,this.Months.Number)
            }
            else
            {
                this.Months.Number--
                this.Months.Name = this.fnmonthday(this.Months.Year,this.Months.Number)
                this.Months.NOD = this.fnNOD(this.Months.Year,this.Months.Number)
            }
            console.log(this.Months)
            this.calculatemd()
        },
    },
    created() //задаются значения сегодняшней даты
    {
        this.Day = this.Data.getDate()
        this.Month = this.Data.getMonth()
        this.Year = this.Data.getFullYear()
        this.Months.Number = new Date(this.Year,this.Month).getMonth(),
        this.Months.Year = new Date(this.Year,this.Month,1).getFullYear()
        this.Months.Name = this.fnmonthday(this.Year,this.Month)
        this.Months.NOD = this.fnNOD(this.Year,this.Month)
        console.log(this.Months.NOD)
        this.calculatemd()
        console.log(this.Months)
    }
};
</script>

<style>
.w
{
    background-color: green;
}
.q
{
    background-color: purple;
}
.e
{
background-color: brown;
}
.flex
{
    width: 500px;
    height: 60px;
    background-color: black;
}
.stack
{
    width: 500px;
    height: 100px;
    background-color: blue;
}
    .label1
    {
        background-color: red;
    }
    .label2
    {
        background-color: green;
    }



</style>
