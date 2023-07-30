# ENTITY COMPONENT SYSTEMS

Game divided into entities and components

## Entities
- Objects in our game like Player, Enemy, DoorTrigger
- Entities still the objects in the game, but only represented with an id.
- Maybe like this
class Entity {
    int id;
}
- but why even do this
- maye an entity is just an id, an components have an id set on them representing which entiy they belong to


## Components
- Entities comprised of components. Components add state/behavior to objects.
- Eg: Transform/Position Component, Animation Component, Collision Component, Sprite/Model Component
- Components are pure data, no behavior. Basically structs with data in them
- Components are organized by data itself rather than by entity, contigious list of data in memory
     - Main difference between object oriented and data-oriented design
- Basically no code like this 
    class Entity {
        list<Component> components
    }

    instead maybe
    component[] components;
- A component might look like:
struct TransformComponent {
    vec3 position;
    vec3 scale;
    quat3 rotation;
    int entityId // If we need this here (see note in entity section)
};

struct VelocityComponent {
    vec3 velocity;
};


## Systems
 - Logic that transform components from one state to the next state
 - Eg: A MovementSystem might update the positions of all moving entities by their velocity since the previous frame
code might look like:
 class MovementSystem {
    public:
        MovementSystem() {
            RequireComponent<TransformComponent>();
            RequireComponent<VelocityComponent>();
        }

        void Update(double deltaTime) {
            for (auto entity: GetEntities()) {
                VelocityComponent& vc = GetComponent(entity.id)
                TransformComponent& tc = GetComponent(entity.id)

                tc.x += vc.x * deltaTime
                tc.y += vc.y * deltaTime
            }
        }
 };

 # Baby's First ECS

 - We have a max number of components an entity can have. Eg: 8
 - then we have a bitset of 8 elements (array of 8 bits) that we call a signature
 - each component has an id of 0...7. This is used as an index into a bitset.
 if an entity has a certain component turned on that index in the bitset is set to 1, otherwise 0.

 Eg:
 TransformComponent has an id 0
 CollisionComponent has an id of 1
 SpriteComponent has an id of 2

The Tank Entity which has a TransformComponent, CollisionComponent  and SpriteComponent will have a bitset of:   1110000
The Player Entity which has a TransformComponent, CollisionComponent and SpriteComponent will have a bitset of:  1110000
The DoorTrigger Entity will have only a Transform Component has a bitset of:                                     1000000

Next when the systems need to work on entities, they query by this bitset
for example:
TheMovementSystem is interested in entities that have the first two bits set in the bitset so they will get back the TankEntity's Transform and Collision component to work with.

Gonna store it in array where each index contains a pointer to a pool
pool = basically array

1st list = contains pointers to pools. Where each index corresponds to a component id. IF TransfromComponent has id = 1, then all the transform components will be stored at the pool in index 1.
Each pool has the same type of component (in this example transform component)
each idex of the pool array corresponds to the entity id. So every pool will have the same type of components (eg one pool will just be all the transformComponents) but every index of every pool will match an entity id.

What this means is that the entity Tank which has a transform component, sprite component, collision component
will be spread out over the array of pools in the following way.

Eg: 
TransformComponent Id = 0
SpriteComponent Id = 1
CollisionComponent Id = 2
Tank EntityId = 7 

components[0] = a pool of all the TransformComponents of all the entities
components[1] = a pool of all the SpriteComponents
components[2] = a pool of all the Collision components

components[0].pool[7] = The tanks TransformComponent
components[1].pool[7] = The tanks SpriteComponent
components[2].pool[7] = The tanks CollisionComponent